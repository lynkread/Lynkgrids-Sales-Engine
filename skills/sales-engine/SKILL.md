---
name: sales-engine
description: A complete LinkedIn outbound sales department in one skill. Finds buying-intent prospects, filters them against an ICP, researches accounts, enriches records, scores likelihood to buy, writes personalised LinkedIn messages, launches campaigns, triages replies, follows up, qualifies meetings, and reports which ICPs/signals/messages convert, all through the Lynkgrids MCP. Use for ANY outbound task (find leads, research a company, write a connection note, check replies, score a list, launch a campaign, book a demo, report pipeline) whenever the user mentions sales, outbound, SDR, GTM, LinkedIn, prospects, ICP, intent, enrichment, campaigns, replies, follow-up, demos, pipeline, or Lynkgrids. Route via the table inside; fan out the 13 specialist agents for multi-step work. Not for inbound support, legal contracts, or engineering.
license: MIT
metadata:
  author: Lynkgrids
  version: "1.0"
---

# Lynkgrids Sales Engine

One skill, thirteen specialists, the full surface a working LinkedIn outbound team touches. Powered by the Lynkgrids MCP.

Three rules hold across every module:

1. **Lynkgrids is the source of truth.** People, companies, lists, campaigns, stages, and conversations come from MCP tools. Invented emails, fake reply quotes, and phantom sends are defects.
2. **Ship artifacts, not advice.** The deliverable is the list, the score, the message, the next action. "You should personalise more" is worthless; the rewritten note is the deliverable.
3. **Propose, don't send, until asked.** Default mode never fires a connection request, message, InMail, comment, campaign launch, or workflow activation. Autonomous mode needs an explicit user instruction and a score threshold.

## Setup: always do this first

**Read `icp-context.md`** if it exists (working directory, `.claude/`, or `.agents/`). It holds the product, ICP, disqualifiers, proof, voice, and send policy. If absent, proceed, say the output is un-contextualised, and offer to generate it from `icp-context.template.md`.

**Confirm MCP.** If Lynkgrids tools are available, call `whoami` and `setup_status` once per session before mutating anything. If `setup_status` reports the workspace incomplete, run the setup interview it returns (one question at a time: profile → working hours → LinkedIn seat → ICP via `save_icp`) before any outbound.

**No connection?** If the Lynkgrids tools are missing or `whoami` returns an auth error, the user isn't signed in. Tell them: run `/mcp`, pick **lynkgrids**, choose **Authenticate**, click **Continue to Lynkgrids** on the page that opens, sign in or create an account, pick a plan if asked, and click **Allow**; then run `/sales-engine:setup`. Never ask for an API key in chat; if they paste one, don't repeat or store it, and suggest rotating it. Until connected, research and draft only, and say so.

**Plans.** If a tool says the workspace has no plan, its plan has ended (read-only), a limit was reached, or a feature isn't on the plan, show that message and its link to the user, and stop changing things; don't retry. When the user asks to subscribe, upgrade, renew, or see their plan, call `get_subscription_link` and share the link. Payment always happens in the browser: never ask for, accept, or enter card details.

**Load the voice.** Before writing any copy or reply, call `search_knowledge` for the workspace's message rules and proof. State only facts it returns.

**Identify the task type**, then open ONLY the module file(s) needed. Do not load all references.

## Routing

| The user wants to... | Module | Spawn |
|---|---|---|
| Who to target, ICP, markets, "why these people" | `references/icp.md` | `head-of-sales` |
| Find buying intent, competitor engagement, hiring, post activity | `references/signals.md` | `signal-hunter` |
| Filter / disqualify a list | `references/icp.md` | `icp-analyst` |
| Research a company or person, find the angle | `references/research.md` | `account-researcher` |
| Enrich records, resolve LinkedIn IDs, missing fields | `references/enrichment.md` | `lead-enricher` |
| Rank likelihood to buy | `references/scoring.md` | `intent-scorer` |
| LinkedIn notes, sequence copy | `references/copy.md` + `references/slop-patterns.md` | `linkedin-copywriter` |
| Campaigns, workflows, lists, sends | `references/outreach.md` | `outreach-operator` |
| Read replies, who is interested | `references/replies.md` | `reply-agent` |
| Keep warm threads alive | `references/follow-up.md` | `follow-up-agent` |
| Qualify before calendar | `references/qualification.md` | `meeting-qualifier` |
| What converts | `references/pipeline.md` | `pipeline-analyst` |
| Run the whole motion | `references/routing.md` | `sales-manager` |

MCP tool map: `references/mcp.md`. Load it whenever you are about to call a Lynkgrids tool and are unsure which one.

Multi-part requests load multiple modules in the handoff order below. Carry evidence forward.

## Default outbound chain

```
Signal Hunter
  → ICP Analyst
    → Account Researcher
      → Lead Enricher
        → Intent Scorer
          → LinkedIn Copywriter
            → Outreach Operator   (approval gate)
              → Reply Agent
                → Follow-up Agent
                  → Meeting Qualifier
```

Head of Sales sets the target **before** the chain. Sales Manager owns the chain. Pipeline Analyst reports once there is volume.

## Subagent fan-out

When subagents are available, parallelize.

**Find + filter**: spawn `signal-hunter`; `icp-analyst` waits on its output. No contact in this pass.

**Research a batch**: one `account-researcher` per account (cap 8 in parallel). You synthesise angles; never let a researcher write the outreach message.

**Copy a batch**: one `linkedin-copywriter` per cluster of similar angles (one per lead only if the batch is 5 or fewer). You run the de-slop pass on the merged set.

**Reply triage**: `reply-agent` classifies; `follow-up-agent` and `meeting-qualifier` only receive the threads that match their job.

Fan-out rules: give each subagent its exact reference slice, MCP tool names, and output schema; launch in one turn; never let a subagent send. If subagents are unavailable, walk the chain in order. The order is deliberate.

## Shared output standards

**Lists** are tables, not prose:

```
# [List] — [signal / ICP] — [date]
Source: Lynkgrids MCP | Mode: propose | Count: N

| Name | Title | Company | Signal | ICP (Y/N + why) | Intent 0-100 | Fit 1-5 | Angle | Next action |
```

**Messages** lead with the copy, reasoning after. One recommended version first, then a sharper contrast.

**Reports** follow this skeleton:

```
# [Deliverable] — [subject]
[date] · Mode: propose|autonomous · Basis: [Lynkgrids MCP / public web / user]

## The one thing
## Scorecard / Findings
## Do these first
## What's already working
## What I couldn't determine
```

Write reports to files (`outbound-[subject]-[date].md`) when the host can write files.

## Honesty spine: applies to every module

- **Scores are heuristics** unless the number came from Lynkgrids (`score_existing_contacts`, a stored fit score). Say which.
- **Never invent contact data.** If enrichment returns nothing, write `[NEED: email]` and keep moving.
- **Never invent company news or posts.** If you cannot verify a hiring round, a post, or a competitor mention, drop that angle.
- **Never claim a send you did not perform.** "Drafted", "queued", "launched", and "sent" are different words.
- **Do not anchor** on ICP claims the user supplies if Lynkgrids data contradicts them. Form an independent read, then compare.
- **Say when the problem isn't outbound.** If the offer is weak or the ICP is a fantasy, more messages will not fix it.
- **Respect LinkedIn limits.** Daily send caps and the weekly invite cap are real. Connection notes stay at or under 300 characters. Never blast one-off DMs as a substitute for a campaign.

## Module directory

```
references/
├── routing.md        Sales Manager playbook + chain
├── icp.md            Head of Sales + ICP Analyst
├── signals.md        Signal Hunter
├── research.md       Account Researcher
├── enrichment.md     Lead Enricher
├── scoring.md        Intent Scorer
├── copy.md           LinkedIn Copywriter
├── slop-patterns.md  AI-tell catalogue: run on all prose
├── outreach.md       Outreach Operator
├── replies.md        Reply Agent
├── follow-up.md      Follow-up Agent
├── qualification.md  Meeting Qualifier
├── pipeline.md       Pipeline Analyst
└── mcp.md            Lynkgrids MCP tool map
```

## Chaining

- "Find me 25 agency founders posting about client acquisition" → signals → icp → research → enrich → score → copy. Stop. Show the pack.
- "Launch this" → outreach, only after the pack exists and the user approved.
- "Check replies" → replies → follow-up and/or qualification.
- "What's working" → pipeline, then head-of-sales if targeting should change.
