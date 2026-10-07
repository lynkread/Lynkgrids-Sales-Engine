---
description: Triage LinkedIn replies in Lynkgrids (pending agent drafts and New Response leads). Classify, flag interested prospects, draft or edit responses. Does not send in propose mode.
argument-hint: "[optional: campaign, owner, or date range]"
---

Triage replies for: $ARGUMENTS

Use the Reply Agent playbook (`skills/sales-engine/references/replies.md`). Pull `pending_replies` and people tagged "New Response". Call `search_knowledge` before drafting. Interested → Meeting Qualifier. Warm / question / objection → Follow-up Agent. Stop / negative → log only. Quote the prospect. Draft, don't send, unless the user approves the exact text.
