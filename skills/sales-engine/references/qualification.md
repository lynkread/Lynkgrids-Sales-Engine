# Qualification: Meeting Qualifier

Qualify before they reach the calendar. Protect the founder's time.

## Bar (default)

A meeting is worth booking when you have:

- **Problem:** they named one we actually solve (quote it).
- **Role:** they can buy, or can get us to the buyer.
- **Timing:** a reason this quarter, not "someday".
- **Fit:** still ICP.

Missing one: ask. Missing three: do not book.

Adapt to `icp-context.md` if the team uses BANT or MEDDIC. Do not force a framework they don't use.

## Process

1. Take the Reply Agent's **interested** (and strong **question**) threads. Read `conversation_history` and `get_person`.
2. Ask the smallest next question that fills the biggest gap. One question.
3. If the bar is met, propose a booking line that matches their CTA. Use a calendar link only if the user provided one.
4. If the bar is not met, hand back to the Follow-up Agent with the gap named.
5. With permission, record it in Lynkgrids: `add_note` with the qualification summary, move the person to the right stage (`list_stages` → `update_person` or `lynkgrids_request`), and `create_task` for the meeting owner.

## Output

```
# Qualification — [Name]
Status: book | ask | pass
Problem quote:
Role:
Timing:
Fit:
Draft:
---
[text]
---
Calendar: [NEED: link] or [link]
CRM updates proposed: [stage → X, note, task]
```

## Guardrails

- Do not surprise-book. The prospect agrees first.
- No six-question discovery forms in LinkedIn DMs.
