---
description: Run the full Lynkgrids outbound chain (find intent, filter ICP, research, enrich, score, write the sequence). Stops before launch unless you already approved or enabled autonomous mode.
argument-hint: "[ICP, signal, geo, or 'autonomous']"
---

Run the Lynkgrids Sales Engine outbound pipeline for: $ARGUMENTS

Follow `skills/sales-engine/SKILL.md` and `references/routing.md`. Spawn `sales-manager` if subagents are available, otherwise walk the chain yourself. Start with `whoami` and `setup_status`.

Default: propose, don't send. Show the copy pack and wait. Only import and launch via the Outreach Operator if the user (in $ARGUMENTS or this session) approved, or set autonomous with a score threshold.

Cap 25 new prospects unless $ARGUMENTS says otherwise.
