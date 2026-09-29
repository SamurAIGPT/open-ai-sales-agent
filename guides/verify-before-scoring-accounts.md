# Verify Company Evidence Before Scoring Accounts

Use this workflow when turning company lookups into an ICP-fit score. It follows the [Company Enrichment skill](../agents/company-enrichment/SKILL.md), whose `company.enrich` capability is marked live and tested.

## Example request

> Enrich these target accounts and rank them for our 50–500 employee SaaS ICP. Show the evidence for each score and leave uncertain accounts unscored.

## Workflow

1. **Define the score before looking up accounts.** Confirm the ICP fields, weights or ranking rules, geography, and any exclusions. If the user has not supplied weights, return a comparison table rather than inventing a numeric score.
2. **Resolve identity.** Normalize each domain. For a company name without a clear domain, surface plausible matches and ask the user to choose; do not silently pick one.
3. **Enrich and preserve provenance.** Call `company.enrich` for each confirmed company. Keep the exact returned value and source/response metadata when available. Do not substitute a planned capability for a live one.
4. **Check field meaning.** Keep employee count, funding stage, and total funding distinct. A missing value stays unresolved. A provider estimate must not be described as a verified count.
5. **Apply the agreed scoring rules.** Score only fields the workflow actually received. Show the evidence behind each point and mark incomplete accounts as partial or unscored. Never backfill a missing field from a different, unverified source without labeling the source and confirming that the field definitions match.
6. **Return the shortlist.** Include the domain, returned firmographic facts, fit rationale, unresolved fields, and any identity ambiguity. Keep all CRM updates and outreach outside this read-only workflow.

## Example output shape

| Domain | Returned evidence | ICP fit | Unresolved fields | Identity confidence |
|---|---|---|---|---|
| `example.com` | Values exactly as returned by `company.enrich` | Apply only the agreed criteria | List missing or unsupported fields | Confirmed / needs review |

The row above is a template, not a real enrichment result. `company.technographics` and `company.funding` are marked not yet live in this repo; do not claim itemized technology detection or round-level funding until those capabilities are verified live.

## Cost and failure handling

Check the current endpoint contract and price in the host/API documentation before a batch. Start with a small bounded sample if the cost is unclear, and report per-row failures. A null or no-match response is not a zero and must not be converted into a low-fit score.

## Approval boundary

This workflow is read-only. Creating or changing CRM records and sending outreach require separate, explicit authorization and a human review of the exact write or message.
