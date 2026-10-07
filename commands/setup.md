---
description: Check the Lynkgrids connection and workspace (sign-in, plan, profile, working hours, LinkedIn seat, ICP) and walk through anything missing, one question at a time.
argument-hint: "[optional: your website]"
---

Set up the Lynkgrids Sales Engine. Context: $ARGUMENTS

1. Call `whoami`.
   - If the Lynkgrids tools are missing, or `whoami` fails with an auth error (401 / unauthorized), the user isn't signed in. Show them the **Sign in** message below and stop. Do not continue setup without a working connection.
2. If `whoami` or `setup_status` says the workspace has no plan, or its plan has ended, call `get_subscription_link` and share the link: the user picks a plan or renews in the browser. Wait for them to say it's done, then run the setup again. Never ask for card details.
3. Call `setup_status`. If setup is incomplete, ask ONE question at a time in this order and save each answer immediately: about you and who you sell to (`save_profile`), working hours and timezone (`save_profile`), LinkedIn seat (give the link from the checklist), email account (optional, mention once), ideal customer profile (`save_icp`).
4. Offer to write `icp-context.md` from `skills/sales-engine/icp-context.template.md` using the answers.
5. When complete, show the slash command menu from the README.

## Sign in

Send this to the user:

> To connect, the Sales Engine needs you to sign in to Lynkgrids.
>
> 1. Run `/mcp`, pick **lynkgrids**, and choose **Authenticate**.
> 2. On the page that opens, click **Continue to Lynkgrids**, then sign in or create an account (and confirm your email).
> 3. If your workspace has no plan, pick one or start a trial. You pay in the browser.
> 4. Click **Allow**, come back here, and run `/sales-engine:setup` again.
>
> Already have a Read & write API key? Use **Use an API key instead** on that page and paste it there, not here.

If the user pastes a key into chat anyway, do not repeat it, store it, or write it to any file. Tell them to use the sign-in above and to rotate the key in Lynkgrids, since it is now in the chat history.
