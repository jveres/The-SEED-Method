# The SEED Method

**The SEED Method** is a development methodology for agent-driven projects.
One human. Any number of agents. The method keeps it manageable.

This playbook is typically stored as `AGENTS.md` in a consuming repo
(repo root). In this source repo, the canonical text is split across
`SEED-Method.md` (kernel + index) and `docs/playbook/` (sections + appendix).

This repo runs on the SEED Method, version: 3.5.1

---

## Principles

These are the foundations. Every rule in this playbook exists to serve them.

1. **SEED gates everything.** No work without direction. The Owner defines what to build; agents don't guess.
2. **Proof over opinion.** Prove it works — don't claim it. Observable evidence is the only currency.
3. **One canonical source.** Every fact has one home. Other files may point to it, never contradict it.
4. **The worklog is the dashboard.** Current state is always readable in one place. If it's not in `WORKLOG.md` or referenced from it, it didn't happen.
5. **Guardrails accumulate.** Learn from mistakes, then lock them in so they can't recur.
6. **Owner decides, agents execute.** Authority is clear. Agents have wide autonomy within bounds — but scope, constraints, and exceptions belong to the Owner.
7. **Stop when stuck.** Don't guess past a blocker. Document it, ask, wait.
8. **Learn both ways.** Capture what went wrong _and_ what went right. Both are institutional knowledge.

---

### How to use this doc

- If you are the **Owner** (human):
  - To set up a project, start at §0 (SEED).
  - To check current status at any time, read `WORKLOG.md` — the first four sections (`## Status`, `## Next actions`, `## Open questions`, and `## Goal`) are the dashboard. Everything you need is in the first screenful.
  - To give feedback at checkpoints, see §6.
  - To close a release, see §10 (pre-sign-off checklist) then §10a (post-sign-off closeout).
- If you are an **Agent**, obey Global rules, then follow phases sequentially. In multi-agent mode, also obey §9.

### Owner responsibilities (summary)

- Provide SEED answers (§8) or confirm an existing `SEED.md`.
- Check project status anytime via `WORKLOG.md` dashboard sections (top of file).
- Respond to checkpoint review guides (§6) with approvals, decisions, and scope changes.
- Declare whether external coordination tooling is in use (so §9 source of truth is unambiguous).
- Surface red-line constraints early (security posture, network/persistence policy, non-negotiable requirements).

---

**SEED → Bootstrap → Build → Prove → Worklog → Adapt**

The **Owner** is the single human directing the project. Agents execute under this playbook on the Owner's behalf.

---

## Global rules (apply to every phase)

1. **SEED-gated:** do not implement before `SEED.md` exists (see §0).
2. **Proof-first quality:** meaningful behavior changes require proof (see §4).
3. **Worklog discipline:** every work chunk/session rewrite-updates the snapshot sections and appends a log entry in `WORKLOG.md` (see §5). In multi-agent mode (§9), contributing agents satisfy this rule by submitting a handoff note (see §9.8 Handoff Note Template) via the coordination mechanism. The coordinator integrates it into `WORKLOG.md`.
4. **Artifact freshness:** `WORKLOG.md` must reflect the current state of the integration baseline (e.g., `main`). In-progress work on branches must be visible via `## Tasks` and ownership/reservations (or the referenced external coordination tool). A handoff entry is required before any handoff to another agent or the Owner.
5. **Runtime stop rule:** if an unexpected blocker arises (tool failure, contradictory requirements, security concern), stop implementation, document the blocker in `WORKLOG.md` → `## Open questions`, and ask the Owner before proceeding. In multi-agent mode (§9), contributing agents satisfy the documentation requirement by posting the blocker via the coordination mechanism (handoff note or task comment). The coordinator integrates it into `WORKLOG.md` → `## Open questions`. Escalate blockers per §9.5 (coordinator first, unless the blocker is scope/constraint/security/exception-level, in which case escalate directly to the Owner).
6. **Idle = housekeeping:** when blocked or waiting (on owner feedback, dependencies, CI, or unclear direction), use the time for housekeeping — not new implementation. See §5a.
7. **Guardrails:** default guardrails (§11) are always active. Project-specific guardrails in `SEED.md` → `## Guardrails` are equally binding. No guardrail may be violated without Owner approval.
8. **Identity:** Agent names are session-scoped and may change on renewal. Owner names and role labels (defined in `SEED.md` → `## Participants`) are the stable identifiers for traceability. Agent names may appear in log entries as session context but must not be relied on for cross-session traceability.

   **Platform metadata:** Agent participants record their platform (`orchestrator · model`) in the `## Participants` table. The table captures the typical/default platform; log entries and handoff notes capture the actual session configuration when it differs. Platform metadata aids debugging, capability attribution, and reproducibility — but does not affect authority or process rules.

   **Role labels vs. names:** In procedural and authority contexts (playbook rules, guardrail attribution, SEED structure, escalation references), use role labels — they encode authority. In operational artifacts (log entries, commits, checkpoint messages, proposals, task discussions), resolve to participant names from `## Participants` — they provide traceability and human readability.

---

## 0) First rule: ask for the SEED (always)

If there is no `SEED.md` in the repo, the first action is to ask the Owner for the SEED using the question script in §8.

Do not start implementation before receiving SEED, **unless**:

- the SEED is already present in chat, and
- you write it to `SEED.md` immediately (preserving Owner wording).

### Deadlock escape hatch (Owner unavailable / cannot answer)

If the Owner is unavailable or cannot answer SEED questions:

1. Stop implementation.
2. Create `SEED_REQUEST.md` containing:
   - the SEED questions (see §8)
   - any proposed assumptions under `## Assumptions (UNAPPROVED)`
3. Wait for Owner input.

### Writing `SEED.md`

After receiving SEED, write `SEED.md` immediately:

- Preserve the Owner's wording.
- Add only necessary clarifications under `## Clarifications`.
- Record approved process adaptations under `## Clarifications` → `### Process knobs`.

### SEED amendments (scope changes)

If Vision or Constraints need to change after `SEED.md` is written:

- Scope changes require Owner approval.
- Record approved changes under `## Amendments` with date and rationale.
- Mention the amendment in the latest `WORKLOG.md` log entry.

---

## Full playbook sections (modular)

Sections §1–§11a are stored in `docs/playbook/` to keep this file smaller
for day-to-day reading and agent ingestion.

- §1: [docs/playbook/01-autonomy-contract.md](docs/playbook/01-autonomy-contract.md)
- §2: [docs/playbook/02-bootstrap.md](docs/playbook/02-bootstrap.md)
- §3: [docs/playbook/03-build.md](docs/playbook/03-build.md)
- §4: [docs/playbook/04-prove.md](docs/playbook/04-prove.md)
- §5–§5b: [docs/playbook/05-worklog.md](docs/playbook/05-worklog.md)
- §6: [docs/playbook/06-adapt.md](docs/playbook/06-adapt.md)
- §7: [docs/playbook/07-audit.md](docs/playbook/07-audit.md)
- §8: [docs/playbook/08-seed-questions.md](docs/playbook/08-seed-questions.md)
- §9: [docs/playbook/09-multi-agent.md](docs/playbook/09-multi-agent.md)
- §10–§10a: [docs/playbook/10-release.md](docs/playbook/10-release.md)
- §11–§11a: [docs/playbook/11-guardrails.md](docs/playbook/11-guardrails.md)
- Appendix templates: [docs/playbook/12-appendix-skeletons.md](docs/playbook/12-appendix-skeletons.md)

---

## 1) Autonomy contract (default)

This section is stored in `docs/playbook/01-autonomy-contract.md`.

See [docs/playbook/01-autonomy-contract.md](docs/playbook/01-autonomy-contract.md).
---

## 2) Bootstrap phase (agents own it)

This section is stored in `docs/playbook/02-bootstrap.md`.

See [docs/playbook/02-bootstrap.md](docs/playbook/02-bootstrap.md).
---

## 3) Build phase (full-speed execution)

This section is stored in `docs/playbook/03-build.md`.

See [docs/playbook/03-build.md](docs/playbook/03-build.md).
---

## 4) Prove phase (oracles over opinions)

This section is stored in `docs/playbook/04-prove.md`.

See [docs/playbook/04-prove.md](docs/playbook/04-prove.md).
---

## 5) Worklog phase (snapshot + log in one file; required)

This section is stored in `docs/playbook/05-worklog.md`.

See [docs/playbook/05-worklog.md](docs/playbook/05-worklog.md).
---

## 5a) Housekeeping (idle time)

This section is stored in `docs/playbook/05-worklog.md`.

See [docs/playbook/05-worklog.md](docs/playbook/05-worklog.md).
---

## 5b) Scratch docs (working documents)

This section is stored in `docs/playbook/05-worklog.md`.

See [docs/playbook/05-worklog.md](docs/playbook/05-worklog.md).
---

## 6) Adapt phase (owner checkpoints)

This section is stored in `docs/playbook/06-adapt.md`.

See [docs/playbook/06-adapt.md](docs/playbook/06-adapt.md).
---

## 7) Playbook audit (optional, trigger-based)

This section is stored in `docs/playbook/07-audit.md`.

See [docs/playbook/07-audit.md](docs/playbook/07-audit.md).
---

## 8) Default question script (when requesting SEED)

This section is stored in `docs/playbook/08-seed-questions.md`.

See [docs/playbook/08-seed-questions.md](docs/playbook/08-seed-questions.md).
---

## 9) Multi-agent coordination (parallel execution contract)

This section is stored in `docs/playbook/09-multi-agent.md`.

See [docs/playbook/09-multi-agent.md](docs/playbook/09-multi-agent.md).
---

## 10) Definition of Release Done (DoRD)

This section is stored in `docs/playbook/10-release.md`.

See [docs/playbook/10-release.md](docs/playbook/10-release.md).
---

## 10a) Post-release closeout (seal + reset)

This section is stored in `docs/playbook/10-release.md`.

See [docs/playbook/10-release.md](docs/playbook/10-release.md).
---

## 11) Guardrails (enforced rules that evolve)

This section is stored in `docs/playbook/11-guardrails.md`.

See [docs/playbook/11-guardrails.md](docs/playbook/11-guardrails.md).
---

## 11a) Practice promotion (best practices that evolve)

This section is stored in `docs/playbook/11-guardrails.md`.

See [docs/playbook/11-guardrails.md](docs/playbook/11-guardrails.md).
---

# Appendix — Skeletons

The skeleton templates are stored in `docs/playbook/12-appendix-skeletons.md`
to keep `SEED-Method.md` smaller for day-to-day reading and agent ingestion.

See [docs/playbook/12-appendix-skeletons.md](docs/playbook/12-appendix-skeletons.md).
