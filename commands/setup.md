---
description: Check the Lynkgrids connection and workspace (API key, profile, working hours, LinkedIn seat, ICP) and walk through anything missing, one question at a time.
argument-hint: "[optional: your website]"
---

Set up the Lynkgrids Sales Engine. Context: $ARGUMENTS

1. Call `whoami`.
   - If the Lynkgrids tools are missing, or `whoami` fails with an auth error (401 / unauthorized), the API key is missing or wrong. Show the user the **Get your API key** message below and stop. Do not continue setup without a working connection.
2. Call `setup_status`. If setup is incomplete, ask ONE question at a time in this order and save each answer immediately: about you and who you sell to (`save_profile`), working hours and timezone (`save_profile`), LinkedIn seat (give the link from the checklist), email account (optional, mention once), ideal customer profile (`save_icp`).
3. Offer to write `icp-context.md` from `skills/sales-engine/icp-context.template.md` using the answers.
4. When complete, show the slash command menu from the README.

## Get your API key

Send this to the user (adapt the shell lines to their OS):

> To connect, the Sales Engine needs your Lynkgrids API key.
>
> 1. Sign up or log in at https://lynkgrids.com
> 2. Go to **Settings → Workspace → API keys** and create a **Read & write** key (it starts with `lgk_live_`).
> 3. Set it in your terminal, then restart Claude Code from a new terminal:
>    - macOS / Linux: `export LYNKGRIDS_API_KEY=lgk_live_your_key` (add it to `~/.zshrc` or `~/.bashrc` to keep it)
>    - Windows PowerShell: `setx LYNKGRIDS_API_KEY "lgk_live_your_key"`
> 4. Run `/sales-engine:setup` again.
>
> Don't paste the key here in chat. It only goes in your environment.

If the user pastes a key into chat anyway, do not repeat it, store it, or write it to any file. Tell them to set it as above and to rotate the key in Lynkgrids, since it is now in the chat history.
