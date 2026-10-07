# Lynkgrids MCP tool map

Hosted server: `https://mcp.lynkgrids.com/mcp`. Auth is browser sign-in (MCP OAuth): in Claude Code, `/mcp` → lynkgrids → Authenticate → **Continue to Lynkgrids** → sign in or sign up → plan if needed → **Allow**. Headless clients can instead send a **Read & write** API key (Settings → Workspace → API keys) as `Authorization: Bearer`. Never paste a key into chat or a file.

Call tools by their live names as the server advertises them. This file is a map, not a contract. If the server lists a different name, use the live one. For any route without a curated tool, read the `lynkgrids://api-guide` resource and use `lynkgrids_request`.

## Identity and setup

| Tool | Use |
|---|---|
| `whoami` | Session start: which user and workspace is connected |
| `setup_status` / `setup_checklist` | Is the workspace ready (profile, hours, LinkedIn seat, ICP)? |
| `save_profile` | About the user, who they sell to, working hours, timezone |
| `save_company_profile` | Company / offer details |
| `save_icp` | The ideal customer profile the search tools use |
| `list_team_members` | Multi-seat workspaces, owner routing |
| `list_linkedin_accounts` | Which LinkedIn seats exist; `linkedinAccountId` is the `account_id` for sends |
| `workspace_overview` | One-call status of contacts, campaigns, agents |
| `todays_plan` | What is queued to go out today |
| `get_subscription_link` | The user's billing link and plan status (none / trial / active / expired). Use for subscribe, upgrade, renew. Payment happens in the browser |

## Knowledge

| Tool | Use |
|---|---|
| `search_knowledge` | Company docs, message rules, voice, proof. Call before writing any copy or reply |
| `list_knowledge` / `add_knowledge` / `learn` | Read or add to the knowledge base (approval for writes) |

## Prospecting (finding people)

| Tool | Use |
|---|---|
| `find_leads` | Fresh LinkedIn search driven by the saved ICP. Returns candidate rows, not imports |
| `list_connections` / `refresh_connections` | The seat's 1st-degree connections |
| `linkedin_search_import` | Import from a LinkedIn / Sales Navigator search URL |
| `search_people` / `search_companies` | Query the CRM (paginated: `{ data, pagination }`) |
| `lookup_person` / `get_person` / `get_company` | One record |
| `list_watchlist` / `resolve_watchlist_item` | Accounts and people being watched for signals |
| `latest_posts` | Recent LinkedIn posts (signal source, comment targets) |

## Lead intake and review

| Tool | Use |
|---|---|
| `import_leads` | Import the picks from `find_leads` / `list_connections`. Each needs a `fitScore` 1-5 and a one-line `reason` |
| `review_leads` / `approve_leads` | Leads waiting for review before outreach |
| `leads_summary` / `leads_link` | Counts and a link to the leads view |
| `score_existing_contacts` | Score contacts already in the CRM against the ICP |
| `retry_failed_leads` | Re-run leads that failed to import / enrich |
| `bulk_import_people` / `create_person` / `create_company` | Raw imports (tag them) |

## Enrichment and records

| Tool | Use |
|---|---|
| `enrich_person` | Pull LinkedIn details onto a person record |
| `resolve_provider_id` | LinkedIn username → `provider_id` needed for sends |
| `update_person` / `update_company` | Patch fields. **`tags` replaces the whole array**: `get_person` first, edit, send the full list |
| `add_note` | Research, qualification, and reply notes on a person |
| `create_custom_field` / `list_custom_fields` | Workspace-specific fields (e.g. intent score) |
| `create_list` / `add_to_list` | Named lists |
| `create_pipeline` / `create_stage` / `list_stages` | Deal pipelines and stages |
| `create_task` / `complete_task` / `list_tasks` | Human follow-ups |

## Outreach

| Tool | Use |
|---|---|
| `draft_outreach` | Draft a connection note / sequence for a lead |
| `launch_campaign` | Start the standard sequence for approved leads (approval gate) |
| `launch_outreach` | One-off outreach for a named set (approval gate) |
| `campaign_progress` | Sent / accepted / replied per campaign |
| `pause_campaign` / `resume_campaign` / `run_campaign_now` / `stop_campaign_run` | Only when the user asks |
| `create_workflow` | Custom drip: steps `connect` / `message` / `inmail` / `email` / `enrich` / `comment`, each with `waitDays`. Create as `draft`; activate only on approval |
| `send_connection_request` | Single invite, note ≤ 300 chars (approval gate) |
| `send_linkedin_message` | 1st-degree DM (approval gate) |
| `send_inmail` | InMail, needs a subject (approval gate) |
| `send_email` | Email, only if the workspace allows it (approval gate) |
| `comment_on_post` / `create_post` | Social touches (approval gate) |

Standard campaign shape: connection request → accepted → message 1 → wait 3 days → message 2 → wait 4 days → message 3 → wait 4 days → message 4 → done. A reply stops the sequence for that person. For a 1st-degree list pass `skipConnect`.

## Replies and follow-ups

| Tool | Use |
|---|---|
| `pending_replies` | Replies the Lynkgrids agents already drafted, with what the prospect wrote |
| `send_reply_draft` | Send a drafted reply, only after a yes on the exact text |
| `conversation_history` | Full thread with a person |
| `search_people` with `tags: ["New Response"]` | Everyone who replied and is not handled yet |
| `list_follow_ups` / `waiting_for_me` | Threads where the next move is ours |
| `unanswered_questions` / `answer_question` | Questions agents could not answer from the knowledge base |

## In-app agents (Lynkgrids AI SDRs)

| Tool | Use |
|---|---|
| `agent_list` / `get_agent` / `agent_settings` | What is already running |
| `create_agent` / `update_agent` | New or changed agent (approval) |
| `run_agent` / `pause_agent` / `resume_agent` / `allow_autonomy` | Only when the user asks |

## Escape hatch

| Tool | Use |
|---|---|
| `lynkgrids_request` | Any method + path documented in `lynkgrids://api-guide` |
| `cannot_do` | Log a request no tool can do, then tell the user plainly |

## Call policy

1. Prefer **read** tools in propose mode.
2. Batch reads. Page `search_people` until `pagination.totalPages`; do not `get_person` in a loop of 50 when a search returns the fields.
3. After a write, read it back (`get_person`, `campaign_progress`) and report IDs.
4. "This user is not a relation" → not 1st-degree. Offer a connection request; do not retry the DM.
5. "Invalid parameters" on a connect is usually the weekly invite cap. Stop invites for the week and say so.
6. On auth errors, stop and tell the user to reconnect the MCP. Do not loop sends.
7. Plan answers (no plan, plan ended / read-only, limit reached, feature not on the plan) come back as plain messages with a billing link. Show them to the user, stop mutating, and don't retry. Never handle card details.
8. Never log or echo the API token.
