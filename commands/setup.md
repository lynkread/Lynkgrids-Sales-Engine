---
description: Check the Lynkgrids connection and workspace (sign-in, profile, working hours, LinkedIn seat, ICP) and walk through anything missing, one question at a time.
argument-hint: "[optional: your website]"
---

Set up the Lynkgrids Sales Engine. Context: $ARGUMENTS

1. Call `whoami`.
   - If the Lynkgrids tools are missing, or `whoami` fails with an auth error (401 / unauthorized), the user isn't signed in. Show them the **Sign in** message below and stop. Do not continue setup without a working connection.
2. Call `setup_status`. If setup is incomplete, ask ONE question at a time in this order and save each answer immediately: about you and who you sell to (`save_profile`), working hours and timezone (`save_profile`), LinkedIn seat (give the link from the checklist), email account (optional, mention once), ideal customer profile (`save_icp`).
3. Offer to write `icp-context.md` from `skills/sales-engine/icp-context.template.md` using the answers.
4. When complete, show the slash command menu from the README.

## Sign in

Send this to the user:

> To connect, the Sales Engine needs you to sign in to Lynkgrids.
>
> 1. Run `/mcp`, pick **lynkgrids**, and choose **Authenticate**.
> 2. A Lynkgrids page opens in your browser. Sign in, or create an account if you don't have one, and approve the connection.
> 3. Come back here and run `/sales-engine:setup` again.
>
> If that page asks for an API key, create one in Lynkgrids under **Settings → Workspace → API keys** (Read & write) and paste it into the browser page, not here.

If the user pastes a key into chat anyway, do not repeat it, store it, or write it to any file. Tell them to use the sign-in above and to rotate the key in Lynkgrids, since it is now in the chat history.
