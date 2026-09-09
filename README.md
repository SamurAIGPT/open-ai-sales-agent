# AI Sales Agent

An AI agent for sales — lead generation, LinkedIn outreach, company enrichment, and email verification — backed by real B2B data APIs.

Part of [Agency Agents OS](https://github.com/Anil-matcha/agency-agents-os), an open ecosystem of specialized AI agents for real business work.

## Related Projects

- [Agency Agents OS](https://github.com/Anil-matcha/agency-agents-os) — the central catalog this repo is part of.
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
| [LinkedIn Outreach](agents/linkedin-outreach/SKILL.md) | Draft and sequence personalized connection/outreach messages (never auto-sends) | Tested |
| [Company Enrichment](agents/company-enrichment/SKILL.md) | Enrich a company name/domain with firmographic data | Tested |
| [Email Verification](agents/email-verification/SKILL.md) | Validate a list of email addresses for deliverability before a campaign | Tested |

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

Company enrichment and email verification are `read-only` — they look up and validate data, nothing is sent anywhere. LinkedIn outreach is `requires-approval-to-publish`: the agent only drafts connection/outreach messages and sequences; it never sends a message on its own. A human must review and approve every send.

## Status and limitations

All four sub-agents are **Tested** (2026-09-09): `company.enrich`, `people.search`, and `email.verify` are live on Muapi's production API and have each been run end-to-end with real inputs (e.g. `company.enrich` against `stripe.com` returned a full firmographic profile; `people.search` and `email.verify` returned real matches). LinkedIn Outreach's drafting step is agent-side (no dedicated API beyond the `company.enrich` lookup it uses for personalization) and is exercised the same way.

## Contributing

See [Agency Agents OS CONTRIBUTING.md](https://github.com/Anil-matcha/agency-agents-os/blob/main/CONTRIBUTING.md).

## License

[MIT](LICENSE)
