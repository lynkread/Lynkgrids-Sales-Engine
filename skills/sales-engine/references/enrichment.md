# Enrichment: Lead Enricher

Fill missing data so the record can actually be contacted. Do not guess.

## What LinkedIn outreach needs

To send anything on LinkedIn, Lynkgrids needs:

- a **seat**: `list_linkedin_accounts` → `linkedinAccountId` is the `account_id`
- the recipient's **`provider_id`**: already on enriched records as `linkedinProviderId`, otherwise `resolve_provider_id` with the username from `linkedin.com/in/<username>` and the `account_id`
- the **degree**: 1st-degree can be messaged; everyone else needs a connection request (or InMail)

Email is optional. It does not block the LinkedIn motion.

## Process

1. For each keeper, list missing fields: LinkedIn URL, `provider_id`, degree, current title, company, email.
2. If the person is not in Lynkgrids yet, they come in through `import_leads` (with `fitScore` and `reason`) after approval, or `create_person` / `bulk_import_people` for raw rows. Tag every import with the source and date.
3. `enrich_person` on each imported id. Read the record back with `get_person`.
4. `resolve_provider_id` for anyone still missing it.
5. If a field does not come back, write `[NEED: email]` and leave it empty. Never pattern-guess `firstname.lastname@company.com` and call it found.
6. Deduplicate. The same person on two rows collapses to one; check `search_people` before creating.
7. Write back with `update_person` when you have a real value and permission to mutate. Remember: `tags` replaces the whole array, so `get_person` first and send the full list.

## Output

```
# Enrichment — [date]
Attempted: N | Enriched: n | provider_id resolved: n | Email found: n | Still missing: n

| Name | Company | LinkedIn | provider_id | Degree | Email | Source | Status |
```

Status is `enriched` | `already_had` | `not_found` | `needs_approval_to_write` | `failed` (offer `retry_failed_leads`).

Hand complete rows to the Intent Scorer.

## Guardrails

- No purchased-list paste without ICP + intent first.
- No storing credentials or tokens.
- Phone numbers are sensitive: include only if Lynkgrids returned them and the send policy allows calls.
