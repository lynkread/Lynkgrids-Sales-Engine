---
description: Check the Lynkgrids workspace is ready for outbound (profile, working hours, LinkedIn seat, ICP) and walk through anything missing, one question at a time.
argument-hint: "[optional: your website]"
---

Set up the Lynkgrids Sales Engine. Context: $ARGUMENTS

1. Call `whoami` and `setup_status`. If the Lynkgrids tools are missing, tell the user to connect the MCP (see README) and stop.
2. If setup is incomplete, ask ONE question at a time in this order and save each answer immediately: about you and who you sell to (`save_profile`), working hours and timezone (`save_profile`), LinkedIn seat (give the link from the checklist), email account (optional, mention once), ideal customer profile (`save_icp`).
3. Offer to write `icp-context.md` from `skills/sales-engine/icp-context.template.md` using the answers.
4. When complete, show the slash command menu from the README.
