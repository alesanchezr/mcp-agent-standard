# mcp-quality

Portable **Agent Plugin** with skills for designing, reviewing, and scaffolding MCP servers — especially knowledge-graph backends with a shared HTTP API for reports and human UIs.

This pack is **guardrails and workflow**, not a running MCP server or graph database. Agents use the skills to build those in *your* project.

## Install

- Open this repo as a Cursor / Agent Plugin project, or install via Cursor Plugins from the package root (where `plugin.json` lives).
- Skills load from `skills/*/SKILL.md` automatically.

## Skills

| Skill | When to use |
|-------|-------------|
| **mcp-quality-server** | Any MCP design or review: envelopes, gates, playbooks, `discovery_path`, discovery attestation, shared domain API |
| **mcp-knowledge-graph** | Graph/ontology tool design (after quality) |
| **mcp-kg-builder** | “Build/maintain a KG MCP for my use case” — interview → confirm model → scaffold core + MCP + API |

There is **no** fourth API-only skill: shared HTTP API guidance lives in the quality skill and builder.

## Authority

- **Agents:** follow the skills. When the long essay and skills disagree, **skills win**.
- **Humans:** read [`docs/mcp-standards.md`](docs/mcp-standards.md) for the narrative quality bar (product appendix at the end is history, not portable requirements). Case study: [`docs/mcp-has-no-quality-standards-we-wrote-ours-so-agents-can-run-our-website.md`](docs/mcp-has-no-quality-standards-we-wrote-ours-so-agents-can-run-our-website.md).
- Sync the essay when you change the contract in skills.

## Frictionless local access (default)

For non-technical builds, scaffolded servers should:

1. Bootstrap a **work session** before serious work and before minting UI links
2. Issue **2-hour** signed UI links tied to `work_session_id` (remint in chat on failure; warn not to forward)
3. After first open, use a **same-day (~8–12h)** browser session
4. Keep UI **read-oriented**; high-blast writes stay on the agent/MCP path with discovery attestation

## Out of scope for this pack

- No bundled graph DB or live MCP process
- No website-v3 / product CMS paths in skills
- No Cursor-only `rules/` / `commands/` packaged in the plugin
