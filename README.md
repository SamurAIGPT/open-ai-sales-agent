# AI Sales Agent

An AI agent for sales — lead generation, LinkedIn outreach, company enrichment, and email verification — backed by real B2B data APIs.

Part of [Agency Agents OS](https://github.com/Anil-matcha/agency-agents-os), an open ecosystem of specialized AI agents for real business work.

## What this covers

This repo is the umbrella for anything an agency or in-house sales team would call "the AI sales agent": turning an ideal-customer-profile description into a verified, enriched prospect list, and drafting the outreach to reach them — without hand-operating a separate tool for each step.

## Sub-agents

| Agent | Does | Status |
|---|---|---|
| [Lead Generation](agents/lead-generation/SKILL.md) | Build a targeted prospect list from an ICP description | Coming Soon |
| [LinkedIn Outreach](agents/linkedin-outreach/SKILL.md) | Draft and sequence personalized connection/outreach messages (never auto-sends) | Coming Soon |
| [Company Enrichment](agents/company-enrichment/SKILL.md) | Enrich a company name/domain with firmographic data | Coming Soon |
| [Email Verification](agents/email-verification/SKILL.md) | Validate a list of email addresses for deliverability before a campaign | Coming Soon |

## Required Muapi APIs

- `company.enrich` — firmographic enrichment (size, funding, tech stack, industry) for a company name or domain.
- `people.search` — prospect/contact search against an ICP description.
- `email.verify` — email deliverability validation for a list of addresses.
- `outreach.draft_message` — personalized outreach/connection message drafting.

See each sub-agent's `SKILL.md` for the specific capabilities it uses.

## Setup

1. Create a Muapi account and API key at [muapi.ai](https://muapi.ai).
2. Review the [Muapi API quickstart](https://muapi.ai) and [OpenAPI schema](https://api.muapi.ai/openapi.json) for the relevant endpoints as they become available.
3. Load the `SKILL.md` for the sub-agent you need into your agent runtime (hosted agent, MCP client, or custom LLM app), or follow it manually.

## Read-only vs. write actions

Company enrichment and email verification are `read-only` — they look up and validate data, nothing is sent anywhere. LinkedIn outreach is `requires-approval-to-publish`: the agent only drafts connection/outreach messages and sequences; it never sends a message on its own. A human must review and approve every send.

## Status and limitations

All four sub-agents are Coming Soon. They depend on B2B enrichment, people-search, and email-verification capabilities that are not yet live on Muapi. This repo defines the intended shape of each agent (inputs, workflow, decision rules, approval boundaries, output format) so implementation can start as soon as the underlying Muapi capabilities ship.

## Contributing

See [Agency Agents OS CONTRIBUTING.md](https://github.com/Anil-matcha/agency-agents-os/blob/main/CONTRIBUTING.md).

## License

[MIT](LICENSE)
