---
title: "MCP Server Standards"
description: "A human-friendly quality bar for building MCP servers — envelopes, gates, playbooks, discovery_path, discovery attestation, shared domain API, and agent education. Domain-agnostic core with a short product appendix."
date: "2026-08-13"
slug: mcp-standards
tags:
  - mcp
  - standards
  - agents
  - engineering
status: draft
---

# MCP Server Standards

This is the quality bar we use when we build or change an MCP server. It is written for **humans** — engineers, reviewers, and anyone who needs to understand *why* tools should behave a certain way.

The Model Context Protocol defines how agents connect and call tools. It does **not** define how those tools should teach blast radius, non-effects, or the next correct call. That gap is ours to fill.

Use this document when you add a tool, change a mutate response, or review an MCP PR. The principles apply to any domain (content, commerce, ops, data, knowledge graphs) — not to one product or site. For the narrative case study behind these ideas, see [MCP Has No Quality Standards…](./mcp-has-no-quality-standards-we-wrote-ours-so-agents-can-run-our-website.md).

When this essay and the installed agent skills in this pack disagree, **skills win for agent behavior**; update skills first and sync this essay in the same change when practical.

---

## What we optimize for

An MCP that **mutates anything that matters** should optimize for:

1. **Correct writes** — agents do the safe thing without memorizing your internal model.
2. **Honest blast radius** — agents know what happened *and* what did not.
3. **An obvious next call** — follow-ups name real tools, not invented ones.
4. **Token efficiency without losing facts** — dense structure beats long essays.

We do **not** optimize for tool count, README-length tool descriptions, or silent "helpfulness" (for example, updating every related resource behind the agent's back without saying so).

---

## The agent loop (mental model)

Every serious mutate should feel like this:

```mermaid
flowchart LR
  discover[Discover_contract_playbook_bootstrap]
  path["discovery_path optional"]
  attest[Attest_on_consequential_call]
  gate[Gate_if_needed]
  mutate[Mutate]
  educate[Warnings_and_side_effects]
  next[Next_actions]
  discover --> path
  path --> attest
  attest --> gate
  gate --> mutate
  mutate --> educate
  educate --> next
  next --> discover
```

| Step | Meaning |
|------|---------|
| Discover | Read the contract (schema, playbook, sample peers, bootstrap / work session) before inventing shape or side effects. Optionally deepen judgment via `discovery_path`. |
| Attest | On high-blast follow-ups (or when a research menu was offered), declare research `completed` or explicitly `skipped` on the consequential call — see Standard 5. |
| Gate | If judgment is needed (destructive/live action, ambiguous target, new enum value, missing tenant), stop and ask — do not hard-fail cryptically. |
| Mutate | Perform the write. |
| Educate | Return structured `warnings` / `side_effects` (paths, non-effects, blast radius). |
| Next | Return `next_actions` with **real** tool names and `args_hint` when you can. |

If any step is "read a paragraph and guess," the design is unfinished.

---

## Standard 1 — Response envelope

Mutating tools share one success/error contract. Do not invent one-off success JSON shapes per tool.

| Shape | When |
|-------|------|
| Success (`ok`) | Write succeeded. Always include `warnings` and `next_actions` (arrays; use `[]` when empty). Optional `side_effects`. |
| Hard error (`fail`) | Something is wrong and retrying the same call will not help without changing inputs. Be clear. Do not pretend there is a safe next tool call. |
| Soft gate (`actionRequired`) | Judgment needed: confirm a destructive/live action, disambiguate a target, supply a tenant/site id, confirm a new enum/taxonomy value, missing discovery attestation, etc. May include `next_actions` for the retry. |

**Always say what did not happen.** Non-effects belong in `warnings` with stable `code`s — not only in a friendly `message` paragraph. Agents skim prose; they trust structured fields.

**Inspection/read tools** that only report state should return `next_actions: []` unless a follow-up is required for correctness.

Implement the envelope in one shared helper module so every mutate tool stays consistent.

---

## Standard 2 — Dense education for agents

Human UI and MCP should teach the **same facts** at different compression:

- **Humans (UI / docs):** Clear, explanatory how-it-works; optional advanced detail; empty states that teach.
- **Agents (MCP):** Short tool descriptions + structured `warnings` / `side_effects` / `next_actions`. Keep payloads efficient **without dropping facts** (which resource, which scope, precedence, non-effects).

A human paragraph often becomes three MCP fields. That is intentional. Shortening a payload by deleting a non-effect is not efficiency — it is lying by omission.

---

## Standard 3 — Guide, don't silently fan out

- Do **not** silently update sibling resources (other locales, environments, bound copies, related records, graph neighbors) unless the tool's contract makes that blast radius explicit *and* the agent can see it in `side_effects` / `warnings`. Prefer guiding the agent with `next_actions` for follow-up writes.
- Prefer **one** tool vocabulary plus a target/scope parameter over parallel tool families (`update_x` vs `update_x_shared`, etc.). Extra names double the hallucination surface.
- When a write has wide blast radius, say so in `reason` / `side_effects` / `warnings` — what else is affected, what still needs a follow-up, what breaks if they skip it.

Surprising multi-resource writes are worse than an extra round trip.

---

## Standard 4 — Gates over opaque errors

Use a soft gate (`actionRequired`) when the agent needs judgment, for example:

- Confirming a **destructive** or **live/production** action
- Choosing among **ambiguous targets** (which resource, which overlay, which environment)
- Supplying a **tenant / project / site** when more than one exists
- Confirming a **new** enum, tag, category, relation type, or other taxonomy value that peers do not already use
- Supplying **discovery attestation** when required (Standard 5)

Gates turn "the model guessed" into "the model asked the principal" (human or orchestrator). Permission to write is not permission to invent product taxonomy or expand blast radius unnoticed.

---

## Standard 5 — Discover before mutate

Dangerous or structural workflows ship with a **playbook** the agent can fetch (explain/topic tools, schema tools, or equivalent) — not only a long tool description. Session bootstrap (playbook + conversation conventions + recent changelog + **work session** when the server supports human UI links) teaches standing habits before the first write.

A solid happy path looks like:

1. Discover tenancy / scope when multiple exist → pass that scope on later calls
2. Start or resume a **work session** when the server requires it (especially before minting human UI links)
3. Read the **contract** — required fields, allowed shapes, how create/update works, observed peer values
4. Read the **playbook** when the write is live, shared, or high blast radius
5. **Sample peers** so new data matches real conventions
6. **Mutate** (with discovery attestation when required)
7. **Verify** with a read or diagnostics tool

Do not make the agent guess which create path applies (draft vs live, shared template vs instance, API-backed vs file-backed). Put that in the contract.

### Optional `discovery_path` (research menu)

Any MCP response may include **`discovery_path`** when the server wants to recommend optional ways to deepen context **before** a consequential next step (decide, mutate, triage, publish, etc.). It is **moment-agnostic** — not tied to one product surface.

| Field | Role |
|-------|------|
| `discovery_path` | Optional research menu (think and/or tools); following items is optional |
| `next_actions` | Real follow-ups needed for correctness |

Shape:

- `goal` — deepen context before acting; not `next_actions`
- `items[]` — ordered; **`kind: "think"`** first (judgment / host-agnostic research; no fake MCP tools), then **`kind: "tool"`** (registered catalog name + `available` + optional hint when capped)
- `non_effects` — e.g. skipping research content does not by itself apply/mutate

Rules:

- Not required on every `ok()`
- Think items capped (≤5), short `why` / `look_for`
- Tool names must be real catalog tools; never invent tools
- Unavailable-but-useful tools stay visible (`available: false` + ask-human / refresh-connector hint)
- Do **not** fold discovery items into `next_actions`
- No resource-specific `args_hint` on discovery tool items — the agent chooses args from the rest of the payload
- Following menu items is optional; the menu itself is **not** a lock on the next tool

### Discovery attestation (param on the next consequential call)

Separately from the menu: when the agent makes a **consequential** call after a research opportunity or on a high-blast write, they must **declare** what they did — including an explicit choice to do no research.

**Require a `discovery` argument when either:**

1. The prior response attached a `discovery_path`, **or**
2. The call is **high-blast** (live apply, delete-with-dependents, merge, publish, new taxonomy / relation type, etc.)

Shape (on the **same** tool — no new acknowledge tool):

```json
{ "status": "completed", "items_used": ["think:check-peers", "tool:list_entity"] }
```

or

```json
{ "status": "skipped", "reason": "already sampled peers this session" }
```

Rules:

- **`skipped` requires a short `reason`** (one line). Empty skip is invalid.
- Explicit `skipped` satisfies attestation; **silent omit does not**.
- Missing or invalid attestation → `actionRequired` or `fail` with a stable code such as `discovery_attestation_required`.
- Not required on every read/`ok()` or low-blast mutate without a prior menu.
- Do not invent parallel tools (`apply` vs `apply_with_discovery`).

---

## Standard 6 — Exact paths and real tool names

- Validation failures should include **exact property or resource paths** when known — not only metaphors ("missing scope," "invalid config"). Agents edit what they can see.
- When a filesystem or config path matters, put the concrete path in `warnings` / `side_effects`.
- `next_actions[].tool` must be a **registered** MCP tool. Never invent tools that "sound right." Prefer `args_hint` so the next call is copy-pasteable.
- Use priorities deliberately (for example `required` | `recommended` | `optional`).

---

## Standard 7 — Read hygiene

Default reads stay small. Unfiltered "list everything" endpoints should return a **minimal sample** or require filters; full dumps need explicit ids/slugs/queries.

Prefer:

- Filters and search on list tools
- Splitting "heavy" views (full documents, previews, analytics) from "light" views (ids, titles, status)
- Not filling the context window with data that cannot change the next decision

Efficiency is not only about writes.

---

## Standard 8 — Shared domain API (MCP + HTTP)

Agents need to explain data (reports); humans sometimes need small UIs. Both should reuse the **same domain core** as MCP tools — not a second, drifting contract.

```text
Domain / KG core  →  MCP tools (agent education, gates, discovery)
                 →  HTTP API (reports, UI, exports) — live or stub
```

### Rules

1. Extract **shared modules** (ontology, query/mutate services, validation, warning codes, id/path shapes). MCP handlers and API routes are thin adapters — not duplicated business logic.
2. **One vocabulary** of operations/resources: API routes map to the same ops as MCP tools (or a clear 1:1 subset). Refuse a parallel UI CRUD dialect with different nouns/fields.
3. **Shared structure:** entity/edge types, list filters, subgraph query params, stable error/warning `code`s. Prefer the same envelope fields on API JSON where they help (`ok` / `fail` / `actionRequired`, `warnings`, `side_effects`); agent-only fields (`discovery_path`, `next_actions`) may be omitted on HTTP.
4. **Scaffolding:** always ship the shared core with MCP. HTTP may start as a **stub** until reports/UI are needed; when they are, enable routes under the same contract — do not invent a new model later.
5. **Mutate policy:** API is **read-first** by default. High-blast writes prefer MCP with attestation/gates. If HTTP high-blast writes are enabled, require the **same `discovery` attestation**.
6. **Auth:** authentication required; read API must not be public by default.
7. **Work session:** servers that mint human UI links must support bootstrap / start-work-session. Agents start a **server-minted** `work_session_id` before serious work and before minting UI links. Session end invalidates session-tied link tokens; agents may extend token lifetime while the session is alive.
8. **Frictionless local auth (default when the human is non-technical or does not care):**
   - Agent-managed **project-local secret** (gitignored); human never configures OAuth.
   - Signed UI links: **2-hour TTL**, bound to **`work_session_id`**; remint in-chat on expiry/failure; warn “do not forward.”
   - After first open: **same-day local browser session (~8–12 hours)** so mid-read navigation does not die with the link.
   - Until expiry, anyone with the link can open it (forwarding = sharing for that window).
   - UI links are **read-oriented**; high-blast changes stay on the agent/MCP path unless the human later confirms UI writes.
9. Multi-user or internet-facing deploy: propose a stronger login; wait for confirm (propose-first).
10. Propose-first before expanding API routes or write surface.

---

## What we refuse (on purpose)

| Refusal | Why |
|---------|-----|
| Parallel tool families for the same write with different targets | One vocabulary + a target/scope parameter. |
| Silent fan-out across siblings / locales / environments / graph neighbors | Agent orchestrates follow-ups via `next_actions`. |
| Inventing new taxonomy values without confirmation | Taxonomy is a product decision. |
| Success payloads missing `warnings` / `next_actions` | Empty arrays are honest; missing keys break the contract. |
| Non-effects only in prose | They get skimmed away. |
| Tool-count vanity | Quality is "next call obvious + non-effects clear." |
| Treating `discovery_path` **menu items** as required next steps or folding them into `next_actions` | Menu is optional judgment. |
| Silent omit of discovery attestation when required | Explicit `completed` or `skipped`+reason; no dedicated ack tool when a param suffices. |
| Empty `skipped` attestation (no reason) | Rubber stamps defeat the audit. |
| Dual UI/MCP contracts with different nouns/fields | Shared domain core. |
| Public anonymous knowledge-base reads by default | Auth required. |
| Blocking non-technical builders on OAuth product choice | Use frictionless local auth default. |
| High-blast API writes without discovery attestation | Same rule as MCP. |
| Minting human UI links without a work session | Session-tied tokens. |

---

## Checklist before you ship a mutate tool

Classic bar first:

- [ ] Returns through the shared success / hard-error / soft-gate envelope
- [ ] Always includes `warnings` and `next_actions` (arrays)
- [ ] Documents side effects and non-effects (stable codes + paths when known)
- [ ] `next_actions` only name real registered tools; `args_hint` where possible
- [ ] No silent multi-resource fan-out; wide blast radius is explicit
- [ ] Soft gates for destructive/live / ambiguous target / missing tenant / new taxonomy — not cryptic failures
- [ ] Playbook, explain, or bootstrap considered when the shape is dangerous
- [ ] Validation errors include exact property paths when known
- [ ] Default reads stay small
- [ ] Agent education is dense; same facts as human docs/UI, not a shorter lie
- [ ] Changelog / conventions version bumped if the agent-facing contract changed

Then discovery (when relevant):

- [ ] Should this response attach `discovery_path`? (consequential next step needing judgment)
- [ ] Think items capped and host-agnostic; no fake MCP tools
- [ ] Discovery tool names are catalog tools; capped tools stay visible with a hint
- [ ] Discovery not folded into `next_actions`; menu skip does not gate
- [ ] Consequential tools that follow a `discovery_path` (or high blast) require `discovery` attestation param
- [ ] `skipped` includes a short reason; omitting the param is not accepted
- [ ] No separate acknowledge/discovery tool

Then shared API / access (when the server exposes reports or UI):

- [ ] Domain logic lives in shared modules used by MCP and API
- [ ] No drifting UI-only schema
- [ ] Auth not public-by-default; non-technical default uses work-session-tied 2h links + same-day browser session
- [ ] Work session bootstrap exists; UI links not minted without a session
- [ ] UI read-oriented unless human confirmed writes; high-blast API writes use attestation if enabled
- [ ] HTTP stub OK until reports/UI are needed — shared core still extracted

---

## In this product (appendix)

This appendix is **historical / product-specific** (a content CMS for agents). It is **not** part of the portable quality bar. Portable skills must not require these paths or CMS nouns.

This product's MCP is a **content CMS for agents** (YAML entries, sections, SEO/meta, shared-layout shells, translations, diagnostics — one or more sites).

- Shared layout: one tool vocabulary + `layout_target` / `confirm_layout_target`; no MCP locale auto-fan-out — guide with `next_actions`.
- Envelope helpers: `mcp-server/lib/respond.ts`. Playbooks: `explain_site` topics under `mcp-server/explain/`. Session start: `bootstrap_agent`.
- **First `discovery_path` consumer:** `list_proposals` when `proposal_id` is set and the proposal is still decidable (`open` | `partial`). Details: `mcp-server/explain/proposals.md`. The core standard above defines the field; proposals only fill which `items` to emit.

---

## Where knowledge lives

| Artifact | Role |
|----------|------|
| This file (`docs/mcp-standards.md` in the portable pack) | Human-friendly standards |
| Agent skills under `skills/` in the portable pack | What agents load; win on conflict with this essay |
| Product-specific rules and MCP code (other repos) | Implementation for one product — study input, not portable requirements |

When standards and implementation disagree in a product repo, fix the implementation or update that product's docs in the same change. In this portable pack, sync the essay when skills change the contract.
