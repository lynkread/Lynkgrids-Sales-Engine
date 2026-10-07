# Lynkgrids Sales Engine: agent entry point

This repo is one sales operating system: an orchestrator (`skills/sales-engine/SKILL.md`), thirteen specialist agents under `agents/`, and the Lynkgrids MCP (`.mcp.json`, `https://mcp.lynkgrids.com/mcp`). Any agent host (Claude Code, Cursor, Codex, ...) can use it.

Read `skills/sales-engine/SKILL.md` first, then load ONLY the module the task needs. Do not load all references at once.

**Before any work:** read `icp-context.md` if it exists (project root, `.claude/`, or `.agents/`). It holds the product, ICP, disqualifiers, proof, voice, and send policy. If absent, proceed, say the output is un-contextualised, and offer to create it from `skills/sales-engine/icp-context.template.md`.

**MCP first:** if Lynkgrids tools are connected, call `whoami` and `setup_status` before anything else, and `search_knowledge` before writing any copy. Do not invent contacts, emails, campaign stats, or replies. If the MCP is missing or returns an auth error, tell the user to sign in through the MCP's browser flow (in Claude Code: `/mcp` → lynkgrids → Authenticate). Never ask for an API key in chat. Until then, research and draft only; never claim a live send happened.

## Routing

| The user wants to... | Load | Spawn |
|---|---|---|
| Decide who to target, ICP, markets, "who should we go after" | `references/icp.md` + `references/routing.md` | `head-of-sales` |
| Find people showing buying intent, competitor engagement, hiring, post activity, warm connections | `references/signals.md` | `signal-hunter` |
| Filter a list, "are these a fit", disqualify, review leads | `references/icp.md` | `icp-analyst` |
| Research a company or person, find the angle | `references/research.md` | `account-researcher` |
| Import, enrich, resolve LinkedIn IDs, missing fields | `references/enrichment.md` | `lead-enricher` |
| Rank / score likelihood to buy | `references/scoring.md` | `intent-scorer` |
| Write LinkedIn connection notes or sequence messages | `references/copy.md` + `references/slop-patterns.md` | `linkedin-copywriter` |
| Launch or manage campaigns, workflows, sends | `references/outreach.md` | `outreach-operator` |
| Read replies, agent drafts, "who is interested" | `references/replies.md` | `reply-agent` |
| Nudge warm leads, stalled threads, "who's waiting on me" | `references/follow-up.md` | `follow-up-agent` |
| Qualify before a meeting, demo-ready | `references/qualification.md` | `meeting-qualifier` |
| What converts, ICP vs source vs message report | `references/pipeline.md` | `pipeline-analyst` |
| Run the whole motion, "work the pipeline", coordinate | `references/routing.md` | `sales-manager` |

Given only a website with no stated task: run the Head of Sales (ICP from the site) then the Signal Hunter. Show the list. Do not contact anyone until asked.

## Handoff chain (default outbound)

1. `signal-hunter`: find intent
2. `icp-analyst`: keep / drop
3. `account-researcher`: angle
4. `lead-enricher`: import + enrich
5. `intent-scorer`: rank
6. `linkedin-copywriter`: message
7. `outreach-operator`: campaign (approval required unless autonomous)
8. `reply-agent`: inbound
9. `follow-up-agent`: keep warm
10. `meeting-qualifier`: meeting or not

`sales-manager` owns the chain. `pipeline-analyst` reports after enough volume. `head-of-sales` resets targeting when conversion is weak.

Carry evidence forward. Re-researching what a previous agent established wastes tokens and coherence.

## Standards that hold in any host

- Score everything scoreable; state the rubric and that scores are heuristics unless Lynkgrids supplied them.
- Ship artifacts (the list, the message, the score, the next action), not advice.
- End every report with what you couldn't determine.
- Never invent emails, phone numbers, job titles, company facts, or reply quotes. Write `[NEED: x]`.
- Never send a LinkedIn message, connection request, InMail, comment, or reply, launch a campaign, or activate a workflow unless the user approved it **or** enabled autonomous mode with an explicit score threshold.
- Never handle credentials. MCP auth is the host's job.
