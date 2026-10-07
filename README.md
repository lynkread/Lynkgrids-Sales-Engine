# Lynkgrids Sales Engine

**A full LinkedIn outbound team, packaged as one Claude Code plugin.**

Thirteen agents, each with one job, all wired to the Lynkgrids MCP. Together they find, research, contact, and qualify prospects on LinkedIn, and nothing goes out until you approve it.

## The team

| Agent | Job |
|---|---|
| 🖤 **Head of Sales** | Decides who to target and why |
| 🟢 **Signal Hunter** | Finds prospects showing real buying intent |
| 🟪 **ICP Analyst** | Filters out bad fits before outreach starts |
| 🔷 **Account Researcher** | Researches every company and prospect |
| ⚪️ **Lead Enricher** | Imports, enriches, and resolves LinkedIn IDs |
| 🔺 **Intent Scorer** | Ranks prospects by likelihood to buy (0-100 and Lynkgrids 1-5 fit) |
| 🔵 **LinkedIn Copywriter** | Writes the connection note and four-message sequence |
| 🟡 **Outreach Operator** | Launches and manages campaigns, workflows, and sends |
| 🟥 **Reply Agent** | Triages replies and agent drafts, flags interested prospects |
| 🟤 **Follow-up Agent** | Makes sure warm opportunities don't go cold |
| 🟩 **Meeting Qualifier** | Qualifies prospects before they reach your calendar |
| 🟣 **Pipeline Analyst** | Tells you which ICPs, sources, and messages convert |
| 🔶 **Sales Manager** | Coordinates everything and keeps the pipeline moving |

## How work moves between them

```
Signal Hunter finds a VP Sales who posted about hiring SDRs
        ↓
ICP Analyst checks they're a fit (and not already a client)
        ↓
Account Researcher finds the angle
        ↓
Lead Enricher imports + enriches them in Lynkgrids
        ↓
Intent Scorer ranks them against the rest of the list
        ↓
Copywriter writes the note + sequence
        ↓
Outreach Operator launches the campaign (after your yes)
        ↓
Reply Agent handles the response
        ↓
Meeting Qualifier moves them toward a call
```

## Install

You need a Lynkgrids workspace with a plan and a connected LinkedIn seat. If you don't have an account yet, you create one, and choose a plan, during sign-in.

### 1. Install the plugin

```
/plugin marketplace add lynkread/lynkgrids-sales-engine
/plugin install sales-engine@lynkgrids-sales-engine
```

### 2. Sign in to Lynkgrids

Run `/mcp`, pick **lynkgrids**, and choose **Authenticate**. Your browser opens the Lynkgrids connect page:

1. Click **Continue to Lynkgrids**.
2. Sign in, or create an account and confirm your email.
3. If your workspace has no plan yet, pick one or start a trial. Payment is handled by Razorpay, in the browser.
4. Click **Allow**.

Lynkgrids creates a Read & write key named "Claude" for the connection and sends you back. Claude Code keeps the access refreshed. You never copy a key.

Already have an API key? Choose **Use an API key instead** on the connect page and paste it there, never into Claude.

### 3. Check it

Run `/sales-engine:setup`. It confirms the connection and your plan, then checks your profile, working hours, LinkedIn seat, and ICP, and asks for anything missing. If you aren't signed in or have no plan, it tells you what to do.

### Other places

- **claude.ai / Claude Desktop:** add a custom connector for `https://mcp.lynkgrids.com/mcp` and click **Connect**. The same sign-in page opens.
- **Cursor, Codex, other agents:** copy `skills/sales-engine/` into your agent's skills directory, add the MCP at `https://mcp.lynkgrids.com/mcp` (browser sign-in), and use `AGENTS.md` as the entry point.

### Fallback: API key

For clients without browser sign-in (CI, headless servers), create a **Read & write** key under **Settings → Workspace → API keys** and send it as a header:

```bash
claude mcp add --transport http lynkgrids-key https://mcp.lynkgrids.com/mcp --header "Authorization: Bearer lgk_live_your_key"
```

If you do this in Claude Code alongside the plugin, disable the plugin's `lynkgrids` server in `/mcp` so the tools don't appear twice. Never commit the key, paste it into chat, or put it in `icp-context.md`.

Without the MCP the agents can still research and draft, but they can't read your CRM, import leads, launch campaigns, or handle replies.

## Plans and billing

The connector works on a workspace with an active plan or trial.

- **No plan yet:** the sign-in page takes you through the plan picker before you click Allow.
- **Plan ended:** the workspace turns read-only. Lookups still work; anything that changes or sends something is refused, with a link to renew.
- **Limits and features:** if an action hits a plan limit, or uses a feature your plan doesn't include, Claude shows the message and a link to upgrade.
- **Upgrade any time:** ask "upgrade my plan" or "what plan am I on?". Claude calls `get_subscription_link` and gives you your billing link and plan status.

Payment always happens in your browser, on the Lynkgrids billing page. Claude never asks for or enters card details. If you came through a Lynkgrids partner, the links point to your partner's app and billing.

## First five minutes

1. Copy `skills/sales-engine/icp-context.template.md` to `icp-context.md` in your project root (or `.claude/`) and fill it in.
2. Run `/sales-engine:today` for a read-only look at your workspace.
3. Then: *"Find 25 people who match my ICP and showed buying intent this week. Show me the list before anyone is contacted."*

Default mode is **propose, don't send**. Nothing goes out on LinkedIn until you say so. Say `autonomous` and give a score threshold if you want the team to run unattended.

## Slash commands

| Command | What it does |
|---|---|
| `/sales-engine:setup` | Check the workspace is ready and fill gaps |
| `/sales-engine:today` | Read-only morning brief: queue, replies, who's waiting on you |
| `/sales-engine:outbound` | Full find → filter → research → enrich → score → write pipeline |
| `/sales-engine:find-leads` | Hunt buying-intent prospects |
| `/sales-engine:research` | Research a company, profile, or list |
| `/sales-engine:replies` | Triage replies and agent drafts |
| `/sales-engine:follow-ups` | Draft follow-ups for warm threads and open tasks |
| `/sales-engine:pipeline` | Report what converts |

## How it fits Lynkgrids

- **Lynkgrids does the sending.** Campaigns run the standard sequence: connection request → accepted → message 1 → +3 days → message 2 → +4 days → message 3 → +4 days → message 4. A reply stops it. These agents choose who goes in and what it says.
- **Your knowledge base is the voice.** The Copywriter and Reply Agent call `search_knowledge` before writing, so your message rules and proof come first.
- **In-app agents stay in charge of their drafts.** The Reply Agent reviews drafts from `pending_replies` and only sends the exact text you approve.
- **LinkedIn limits are respected.** Daily caps and the weekly invite cap are treated as hard stops, not errors to retry.

## Design principles

- **Progressive disclosure.** The router loads first. Each task loads only the module it needs.
- **Agent-native.** Multi-step work fans out across the 13 specialists when the host supports subagents.
- **The MCP is the source of truth.** People, campaigns, stages, and conversations come from Lynkgrids, never invented.
- **No executable code.** Markdown plus a hosted MCP URL. Nothing to audit before you trust it.
- **Honesty spine.** No fake emails, no fake proof, no silent sends. Gaps are labelled `[NEED: x]`.

## License

MIT
