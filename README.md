# AI Sales Agent

An AI agent for sales — lead generation, ad-based prospecting, LinkedIn outreach, company enrichment, and email verification — backed by real B2B data APIs.

Part of [Open Business Agents](https://github.com/Anil-matcha/open-business-agents), an open ecosystem of specialized AI agents for real business work.

## Related Projects

- [Open Business Agents](https://github.com/Anil-matcha/open-business-agents) — the central catalog this repo is part of.
- [ai-research-agent](https://github.com/SamurAIGPT/ai-research-agent) — deeper web research on a prospect/company beyond enrichment lookups.
- [ai-marketing-agent](https://github.com/SamurAIGPT/ai-marketing-agent) — nurtures leads this repo generates.
- [MuAPI MCP docs](https://muapi.ai/docs/mcp) — connect this repo's `SKILL.md` files via MCP.
- [MuAPI Agent Skills](https://muapi.ai/docs/agent-skills) — background on the `SKILL.md` pattern this repo uses.
- [MuAPI access keys](https://muapi.ai/access-keys) — create the API key this agent needs.

## What this covers

This repo is the umbrella for anything an agency or in-house sales team would call "the AI sales agent": turning an ideal-customer-profile description into a verified, enriched prospect list, and drafting the outreach to reach them — without hand-operating a separate tool for each step.

## Sub-agents

| Agent | Does | Status |
|---|---|---|
| [Lead Generation](agents/lead-generation/SKILL.md) | Build a targeted prospect list from an ICP description | Tested |
| [Ad-Based Prospecting](agents/ad-based-prospecting/SKILL.md) | Find companies actively running ads as a warm, budget-confirmed lead signal | Blueprint |
| [LinkedIn Outreach](agents/linkedin-outreach/SKILL.md) | Draft and sequence personalized connection/outreach messages (never auto-sends) | Tested |
| [Company Enrichment](agents/company-enrichment/SKILL.md) | Enrich a company name/domain with firmographic data | Tested |
| [Email Verification](agents/email-verification/SKILL.md) | Validate a list of email addresses for deliverability before a campaign | Tested |

## Required Muapi APIs

- `company.enrich` — firmographic enrichment (size, funding, tech stack, industry) for a company name or domain.
- `people.search` — prospect/contact search against an ICP description.
- `email.verify` — email deliverability validation for a list of addresses.
- `outreach.draft_message` — personalized outreach/connection message drafting.
- `ads.ad_library_search` — public ad-transparency library lookup, used by Ad-Based Prospecting to find companies with active ad spend (shared with [`ai-ads-agent`](https://github.com/SamurAIGPT/ai-ads-agent)/[`ai-competitor-intelligence-agent`](https://github.com/SamurAIGPT/ai-competitor-intelligence-agent)).
- `company.technographics` — detect a company's website tech stack, or find companies using a given technology (reverse lookup). *(coded 2026-09-17, not yet live)*
- `company.funding` — a company's funding rounds and financing events, or the latest rounds across companies. *(coded 2026-09-17, not yet live)*
- `company.buying_signals` — buying/intent signals detected for a company. *(coded 2026-09-17, not yet live)*
- `company.job_postings` / `company.headcount_growth` — hiring activity and headcount growth as buying-signal/timing filters. *(coded 2026-09-17, not yet live)*
- `people.rank_decision_makers` — rank a company's decision-makers to surface the best-fit buyer contact. *(coded 2026-09-17, not yet live)*
- `linkedin.company_profile` / `linkedin.people_search` / `linkedin.employees` — read-only LinkedIn company page, member profile, and employee-roster lookups. *(coded 2026-09-17, not yet live)*

See each sub-agent's `SKILL.md` for the specific capabilities it uses.

## Setup

1. Create a Muapi account and API key at [muapi.ai](https://muapi.ai).
2. Review the [Muapi API quickstart](https://muapi.ai) and [OpenAPI schema](https://api.muapi.ai/openapi.json) for the relevant endpoints as they become available.
3. Load the `SKILL.md` for the sub-agent you need into your agent runtime (hosted agent, MCP client, or custom LLM app), or follow it manually.


## Using with an AI agent

Every sub-agent's `SKILL.md` is model- and runtime-agnostic — it's plain Markdown, so it works with any LLM agent, not just Claude. Two integration paths:

**As an MCP connection (the agent gets live Muapi tools):**

Muapi runs an MCP server at `https://api.muapi.ai/mcp` that any MCP-compatible client can connect to — Cursor, Windsurf, Claude, or your own custom agent.

- **Cursor / Windsurf / other clients with a header field:** connect to `https://api.muapi.ai/mcp` with an `Authorization: Bearer YOUR_MUAPI_KEY` header.
- **claude.ai / Claude Cowork / other connector UIs with no header field:** use the URL-embedded key form instead, `https://api.muapi.ai/mcp/YOUR_MUAPI_KEY`, via Settings → Connectors → Add custom connector.
- **Claude Code / Claude Desktop:** `claude mcp add muapi -e MUAPI_API_KEY=YOUR_MUAPI_KEY -- muapi mcp serve` (uses the muapi CLI's stdio transport — Claude Code's HTTP MCP client doesn't reliably inject tools).

Full setup details for every client: [muapi.ai/docs/mcp](https://muapi.ai/docs/mcp).

**As agent instructions (any LLM follows the workflow directly):**

Drop a sub-agent's `SKILL.md` into a Claude Code project's `.claude/skills/` directory, paste it into a custom-GPT/Project's system instructions, hand it to an autonomous agent framework as a tool spec, or attach it directly in a chat conversation — then ask the agent to follow it.

## Read-only vs. write actions

Lead Generation, Ad-Based Prospecting, Company Enrichment, and Email Verification are `read-only` — they look up, source, and validate data, nothing is sent anywhere. LinkedIn Outreach is `requires-approval-to-publish`: the agent only drafts connection/outreach messages and sequences; it never sends a message on its own. A human must review and approve every send.

## Status and limitations

Four sub-agents are **Tested** (2026-09-09): `company.enrich`, `people.search`, and `email.verify` are live on Muapi's production API and have each been run end-to-end with real inputs (e.g. `company.enrich` against `stripe.com` returned a full firmographic profile; `people.search` and `email.verify` returned real matches). LinkedIn Outreach's drafting step is agent-side (no dedicated API beyond the `company.enrich` lookup it uses for personalization) and is exercised the same way. **Ad-Based Prospecting is Blueprint (added 2026-09-10)**: `ads.ad_library_search` is coded but only its Meta leg is confirmed working — Google/TikTok/LinkedIn aren't yet functional (same aggregator vendor-key blocker tracked in `ai-competitor-intelligence-agent`/`ai-ads-agent`); `company.enrich` used within this sub-agent is already live and tested.

**2026-09-17 addition, not yet reflected in the Tested status above:** Lead Generation, Company Enrichment, and LinkedIn Outreach now also reference `company.technographics`, `company.funding`, `company.buying_signals`, `company.job_postings`, `company.headcount_growth`, `people.rank_decision_makers`, and `linkedin.company_profile`/`linkedin.people_search`/`linkedin.employees` in their workflows. These are code-complete server-side but **not yet live** on Muapi's production API (no DB sync/`seed_skills` run yet, and several endpoint ids are still unverified best-effort guesses pending a live token call) — each affected sub-agent's own `SKILL.md` states this plainly. The pre-existing Tested capabilities on these same sub-agents (`company.enrich`, `people.search`, `outreach.draft_message`) are unaffected and remain live.

## Guides

- [Verify company evidence before scoring accounts](guides/verify-before-scoring-accounts.md) — preserve source, uncertainty, and agreed scoring rules.

## Contributing

See [Open Business Agents CONTRIBUTING.md](https://github.com/Anil-matcha/open-business-agents/blob/main/CONTRIBUTING.md).

## License

[MIT](LICENSE)
