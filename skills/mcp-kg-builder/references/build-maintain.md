# Build and maintain checklist

After human confirm of the model proposal:

## Scaffold

- [ ] Shared domain core (ontology, query/mutate, validation, codes) — used by MCP and HTTP
- [ ] MCP tools with shared envelope helpers (`ok` / `fail` / `actionRequired`)
- [ ] `discovery_path` where consequential; `discovery` attestation on menu-or-high-blast mutates
- [ ] `explain_ontology` (or equivalent playbook) + conventions
- [ ] **`bootstrap_agent` / start-work-session** minting `work_session_id`
- [ ] HTTP stub or read API with same ops/params; not public by default
- [ ] Gitignored project secret; link minting helper (2h, session-tied)
- [ ] Same-day browser session after first UI open
- [ ] Cursor MCP config wired to the server
- [ ] Agent changelog habit for contract changes
- [ ] UI read-oriented; high-blast writes on MCP (+ attestation)

## Runtime habits to document for agents

- Start work session before serious work and before minting UI links
- Remint links on expiry/failure in the same chat
- Warn humans not to forward magic links
- Propose-first before growing tools, discovery menus, attestation rules, API routes, or entity/relation types

## Maintain

1. Interview the delta
2. Propose ontology/API/tool/`next_actions`/`discovery_path` changes
3. Wait for confirm
4. Implement + bump changelog
5. If enabling high-blast HTTP writes: add discovery attestation on those routes in the same change

No stack-specific graph driver is required by this pack; pick storage from the interview and keep the vocabulary portable.
