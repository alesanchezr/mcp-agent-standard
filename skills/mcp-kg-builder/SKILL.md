---
name: mcp-kg-builder
description: >-
  Builds or maintains a knowledge-graph MCP server for a human use case:
  interviews for domain model, proposes ontology and tools, scaffolds shared
  domain core plus MCP and HTTP API, work-session bootstrap, and frictionless
  local UI links. Trigger on "build me an MCP", "knowledge graph for …",
  "model my domain", "follow up with clients", scaffold or maintain KG MCP,
  or wire Cursor to a new graph server.
---

# MCP knowledge-graph builder

## Hard require

Apply **`mcp-quality-server`** and **`mcp-knowledge-graph`** first. Load them if missing. Do not invent a parallel quality bar.

## Loop (do not skip interview)

1. **Interview** until the use case is clear (see `references/interview.md`). Example: “follow up with clients” → Client, Interaction, FollowUp/Task — not a blank generic graph.
2. **Auth:** if the human is non-technical or declines to choose → apply frictionless local default (work session, 2h session-tied links, same-day browser session, read-oriented UI). Do **not** quiz them on OAuth. Escalate to multi-user login only when they want sharing/production — propose-first.
3. **Propose** ontology + tool map + API surface (live reads or stub) + access model. **Wait for human confirm.** Do not code the ontology before confirm. Gate new types/relations the same way.
4. **Only after confirm:** scaffold in the consumer project — shared core, MCP (envelopes, tools, playbooks, **bootstrap/work_session**), HTTP stub or read API, gitignored secret, link minting. Wire Cursor MCP config. Changelog when the agent-facing contract changes.
5. **Maintain:** re-interview deltas; propose model/API/tool/`discovery_path`/attestation/table changes; confirm; then implement. Same quality bar.

## Teach the runtime agent

- Start work session before serious work and before minting UI links
- Remint UI links on expiry/failure in chat; warn not to forward
- UI read-oriented; high-blast via MCP + discovery attestation
- Propose-first before growing next_actions, discovery menus, attestation rules, API routes, or entity types

## Deepen

- `references/interview.md`
- `references/model-proposal.md`
- `references/build-maintain.md`
