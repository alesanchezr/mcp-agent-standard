# Model proposal (human sign-off)

**Refuse to scaffold ontology/tools until the human confirms this proposal.**

Present a short, readable proposal:

## 1. Use case (one paragraph)

Restate the outcome in their words.

## 2. Ontology

| Type | Purpose | Key properties |
|------|---------|----------------|
| … | … | … |

| Relation | From → To | Notes |
|----------|-----------|-------|
| … | … | … |

Call out anything new vs empty graph (gates apply).

## 3. Tool map

List MCP tools (list/get/upsert/link/unlink/query_subgraph/explain_ontology/bootstrap_agent) and which high-blast tools need `discovery` attestation.

## 4. API surface

- Shared core modules
- HTTP: stub vs read routes for reports/UI
- Auth: frictionless local default unless they asked for multi-user
- UI: read-oriented unless they confirmed writes

## 5. Work session + links

- Bootstrap returns `work_session_id`
- UI links: 2h TTL, tied to session; remint on failure; warn not to forward
- Browser session ~8–12h after open

## 6. Explicit asks

End with: “Confirm to proceed, or change types/relations/API/auth.”

Do not start coding until they confirm (or give a clear delta you re-propose).
