# Playbooks, bootstrap, work session, changelog

Pattern-level guidance (no product-specific paths).

## Playbooks

Dangerous or structural workflows ship a **fetchable playbook** (explain/topic tool, schema tool, or equivalent) — not only a long tool description. Playbooks teach:

- When to use which write path
- Blast radius and non-effects
- Required confirms / gates
- How to verify after mutate

## Bootstrap

Session start should teach standing habits before the first write, for example:

- Conversation conventions (how to confirm, how to stop)
- Pointers to playbook topics
- Recent agent-facing changelog (what changed in the contract)

Keep bootstrap dense. Deep facts belong in playbooks or payloads, not a novel.

## Work session

When the server exposes human UI/report links (or the builder scaffolds them):

- Expose **bootstrap / start-work-session** that returns a server-minted `work_session_id`
- Agent **must** start (or resume) a work session before serious mutates and **before minting UI links**
- UI link tokens include `work_session_id`; session end invalidates them
- Agent may extend link TTL while the session is alive

Work session is part of Discover for UI-capable servers — not optional ceremony after the fact.

## Changelog habit

When the agent-facing contract changes (tools, envelopes, playbooks, conventions, attestation rules, API routes):

- Append a short changelog entry agents can see at bootstrap
- Bump any conventions version if you version conventions
- Remind humans to refresh/reconnect MCP when the tool list changes

## Propose-first growth

Before growing `next_actions` catalogs, `discovery_path` items, attestation requirements, API routes, or ontology types: **propose to the human, wait for confirm, then change** — same change updates changelog.
