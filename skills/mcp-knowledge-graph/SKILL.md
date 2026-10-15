---
name: mcp-knowledge-graph
description: >-
  Designs MCP tools over knowledge graphs, ontologies, entities and edges, and
  graph RAG — mapping the MCP quality bar to graph CRUD, relation hygiene, and
  shared HTTP API shapes. Use when the user mentions knowledge graph MCP,
  ontology tools, entity/edge CRUD, graph RAG, or subgraph query tools.
---

# MCP knowledge graph

## Hard require

Apply **`mcp-quality-server`** first. If it is not loaded/applied, load it and follow it before continuing. Do **not** invent a parallel quality bar inside this skill.

## Graph-specific rules

1. **No silent neighbor cascade** — linking or deleting must not quietly rewrite unrelated neighbors; say non-effects in `warnings` / `side_effects` and guide follow-ups with `next_actions`.
2. **Gate new relation types and ontology terms** — treat new `rel_type` / entity types like new taxonomy (`actionRequired` / human confirm).
3. **Exact ids** — validation and education use concrete entity/edge ids and property paths.
4. **Read hygiene** — default `list_entity` is filtered/small; heavy `query_subgraph` / neighborhood views are explicit, not the default list.
5. **Attestation** — high-blast graph writes (delete-with-dependents, merge, new relation type, publish-to-live graph) require `discovery` param (completed or skipped+reason), on MCP and on HTTP if those writes are enabled there.
6. **API parity** — HTTP list/get/query params match MCP tool shapes (shared domain core).

## Deepen

- `references/kg-mapping.md` — Standard → KG table
- `references/tool-shapes.md` — suggested vocabulary + scope params
- `references/examples.md` — sample envelopes
