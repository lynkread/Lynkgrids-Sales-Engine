# Contributing

This plugin is markdown plus a hosted MCP URL. Keep it that way.

- One job per agent. If a specialist starts doing another specialist's job, split it back.
- Load one reference module per task. Do not dump the whole `references/` folder into a skill body.
- MCP tool names in `skills/sales-engine/references/mcp.md` must match the live Lynkgrids server. If a tool is renamed, update that file first, then grep `agents/` and `references/` for the old name.
- The default send policy is propose-don't-send. Autonomous mode is opt-in and must stay opt-in.
- Never commit `icp-context.md`, API keys (`lgk_live_...`), or workspace-specific data.
