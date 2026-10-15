# Shared domain API (MCP + HTTP)

## Architecture

```text
Domain / KG core  →  MCP tools (envelopes, gates, discovery)
                 →  HTTP API (reports, UI, exports) — live or stub
```

- Shared modules: ontology, query/mutate, validation, warning codes, ids
- Thin adapters only — no duplicated business logic
- One vocabulary of ops; refuse a drifting UI-only schema
- Same list/get/query params and stable `code`s on both faces
- Agent-only fields (`discovery_path`, `next_actions`) may be omitted on HTTP
- Always extract the core with MCP; HTTP may start as a **stub** until reports/UI are needed

## Mutate policy

- API **read-first** by default
- High-blast writes prefer MCP (+ discovery attestation)
- If HTTP high-blast writes are enabled: **same attestation rule**

## Auth (portable rules)

- Auth required; **not public by default**
- Mechanism is project-specific; do not block non-technical humans on OAuth design

### Frictionless local default

When the human is non-technical or does not care:

1. **Work session:** MCP exposes bootstrap / start-work-session. Agent starts a server-minted `work_session_id` before serious work and **before minting UI links**. Session end invalidates session-tied tokens; agent may extend TTL while session is alive.
2. **Project secret:** gitignored local file; agent creates/manages it.
3. **UI link tokens:** signed, **2-hour TTL**, bound to `work_session_id`. Append to report/UI URLs. Agent warns “do not forward.” Remint proactively in chat on expiry/failure. Clear expired-page message OK.
4. **Browser session:** after first successful open, mint **same-day (~8–12 hour)** local browser session so mid-read navigation does not depend on the link still being fresh.
5. **Forwarding:** until expiry, anyone with the link can open it (honest sharing window).
6. **UI power:** links are **read-oriented**. High-blast changes stay on agent/MCP. Unlock UI writes only with explicit human confirm later.
7. Multi-user / public deploy: propose stronger login; wait for confirm (propose-first).

## Propose-first

Before growing API routes, write surface, or attestation requirements: propose to the human, wait, then implement + changelog.
