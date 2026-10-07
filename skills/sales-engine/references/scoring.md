# Scoring: Intent Scorer

Rank by likelihood to buy **now**. Fit without timing is a nurture lead, not a first touch.

## Rubric (0–100, heuristic unless Lynkgrids supplied a score)

| Band | Score | Lynkgrids fitScore | Meaning |
|---|---|---|---|
| In-market, ICP, fresh trigger, right title | 80–100 | 5 | First touch this week |
| ICP + real trigger, title is champion not buyer | 65–79 | 4 | First touch, CTA = intro/question |
| ICP, weak or stale signal | 45–64 | 3 | Nurture / wait; do not spend invites |
| Weak fit | 25–44 | 2 | Do not contact |
| Off-ICP or no signal | 0–24 | 1 | Do not contact |

`import_leads` takes a `fitScore` of 1–5 plus a one-line `reason`. Use the column above so the two scales always agree. The reason should be the signal, not the score ("Founder of a 24-person agency, posted about client churn on 2 Oct").

### Score parts (weights)

- ICP fit: 30
- Title buying power: 20
- Signal strength + freshness: 35
- Angle quality (researcher confidence): 15

If Lynkgrids already holds a score for the person (`score_existing_contacts`, a stored fit score, a custom field), **start from that** and adjust with the rubric. Show both numbers.

## Process

1. Take researched keepers.
2. Score each with the four parts visible, never a naked 73.
3. Sort descending. Recommend a cut line (default: first touch at 65 or above, unless `icp-context.md` says otherwise).
4. Flag collisions: already in a campaign, already messaged (`conversation_history`), tagged New Response, existing client. Those are not new outbound.

## Output

```
# Intent ranking — [date]
Cut line: [n]
Basis: heuristic + [Lynkgrids scores if any]

| Rank | Name | Company | Total | Fit 1-5 | ICP/30 | Title/20 | Signal/35 | Angle/15 | Cut | Note |
```

Rows at or above the cut go to the Copywriter. Rows below go to a named nurture list (`create_list` / `add_to_list` after approval). Do not delete them and do not message them.

## Guardrails

- Do not average a 12-person sample into "this ICP converts". The Pipeline Analyst owns conversion claims.
- Do not boost a score because someone already wrote a clever line. Copy is downstream.
