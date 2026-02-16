<!-- Generated file: dist/AGENTS.md. Source: SEED-Method.md + docs/playbook/*.md -->

# The SEED Method

**The SEED Method** is a development methodology for agent-driven projects.
One human. Any number of agents. The method keeps it manageable.

This playbook is stored as `AGENTS.md` in the repo root and is the canonical source of truth for agent behavior.

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



## 1) Autonomy contract (default)

Unless `SEED.md` restricts it, agents are authorized to:

- choose tech stack and minimal architecture
- initialize git and set up repo structure
- create/modify any files needed to deliver the Vision
- add tooling/build steps if not forbidden by Constraints
- refactor for maintainability
- run full-speed end-to-end delivery

Agents must ask the Owner before:

- violating any SEED constraint
- expanding scope beyond the Vision
- adding external dependencies when dependency policy is unclear or restrictive
- making user-facing product/design decisions that are ambiguous
- introducing security/privacy risk, network access, or data persistence when not explicitly allowed

### Security hard rule (non-negotiable)

- Never commit sensitive data (credentials, secrets), ensure they are ignored by git.

---


## 2) Bootstrap phase (agents own it)

**Goal:** convert `SEED.md` into a runnable, provable project with minimal overhead.

If git is not initialized and not forbidden by Constraints, initialize it during Bootstrap.

Bootstrap deliverables (create what's needed for the SEED):

- `README.md` with minimal "how to run" + "how to prove"
- initial runnable artifact (even a skeleton)
- proof mechanism appropriate to the project (see §4)
- `WORKLOG.md` (required)
- plan + tasks (required; see below)
- initial commits (if git is available)

### Plan + tasks (required, but lightweight)

Location is inside `WORKLOG.md`:

- `## Plan` (short, sequential)
- `## Tasks` (numbered, small, verifiable; dependency annotations allowed, e.g., "blocked by Task 1")

Notes:

- If SEED implies minimalism (e.g., "single-file", "no build step"), keep ceremony proportional — but `WORKLOG.md` is always required.
- Avoid agent-framework vendor scaffolding unless the Owner explicitly requests it. Standard project tooling (e.g., `.github/`, CI configs) is fine.

---


## 3) Build phase (full-speed execution)

Work in increments ("chunks") that are:

- **verifiable** (proof improves each chunk)
- **revertible** (clean commits or clear rollback)

A chunk is the smallest unit of work that produces a verifiable change. Aim for 1–3 tasks per chunk. If a chunk takes more than one focused session, consider splitting.

In multi-agent mode, a "chunk" maps to one integration event into the baseline (e.g., one merged PR or one coordinated merge set). Individual contributing-agent sessions are captured as handoff notes (§9.8), not as separate chunks.

Prefer finishing the delivery over adding extras not required by SEED.

### 3.1 Chunk Definition of Done (DoD)

A chunk is "Done" only when:

- the deliverable matches SEED scope for that chunk
- proof exists/updated for changed behavior (tests/self-test/checklist)
- proof has been executed and the result recorded:
  - **local run:** command + exit code / key output
  - **CI run:** workflow name + run identifier or link + pass/fail
  - **manual check:** date + pass/fail + notes
- no guardrail violations (default §11 + project-specific in `SEED.md` → `## Guardrails`)
- `WORKLOG.md` snapshot sections are updated (dashboard: status, next actions, open questions; detail: plan/tasks status, proof, decisions, working set)
- `WORKLOG.md` has a new log entry for the chunk
- any hard-won lessons from this chunk are recorded in `WORKLOG.md` → `## Lessons`
- any effective practices discovered in this chunk are recorded in `WORKLOG.md` → `## Practices`

### 3.2 Recovery play (failed chunk)

If a chunk fails partway and cannot be quickly fixed:

1. Revert to the last known-good commit (or clearly mark partial changes).
2. Document the failure in a `WORKLOG.md` log entry (what failed, why, what was attempted).
3. Update `WORKLOG.md` → `## Open questions` with the failure context.
4. Consider whether the failure should become a guardrail — if the mistake is likely to recur, propose a new guardrail at the next owner checkpoint (see §11 → Promotion).
5. Check whether anything _did_ work well before the failure — record those in `## Practices` even if the chunk overall failed.
6. Re-plan: adjust tasks or approach before retrying.

---


## 4) Prove phase (oracles over opinions)

A change is **meaningful** if it alters observable output, user-facing behavior, API contract, or data handling. Refactors that preserve all observable behavior do not require new proof but must not break existing proof.

**Examples of meaningful changes (proof required):**

- changing CLI flags, arguments, or exit codes
- altering API response shape or status codes
- modifying DB schema or query behavior
- changing auth/session handling
- changing file formats, serialization, or default configs
- adding or changing network access or data persistence

**Examples of usually non-meaningful changes (no new proof needed, but existing proof must stay green):**

- renaming/refactoring with identical outputs
- formatting, comments, documentation-only edits
- reordering internal logic without changing observable behavior

For each meaningful behavior, there must be proof.

Preferred:

- automated tests

Acceptable when automation isn't practical:

- built-in self-test mode
- explicit manual checklist steps (clear, reproducible)

Proof must be:

- easy to run
- documented in `README.md` (canonical); referenced from `WORKLOG.md` as pointer only (see §5)
- kept in sync with behavior changes

### When proof cannot run

If proof infrastructure is broken (CI down, tooling unavailable, environment issue), the agent must:

1. Document in `WORKLOG.md` → `## Open questions` as `[risk]`. In multi-agent mode, contributing agents post this via the coordination mechanism; the coordinator integrates it into `WORKLOG.md`.
2. Not merge to main or close tasks until proof is run and recorded.
3. Commits may continue on a branch — but the branch is not merge-eligible until proof passes.
4. If no branch work is possible, enter idle housekeeping (§5a).

The Owner may explicitly approve merging without proof as a one-time exception (see §6.1).

### Non-functional requirements gate (optional)

When SEED Constraints specify non-functional requirements (performance budgets, accessibility standards, security posture, privacy rules), add explicit proof or checklist items for those requirements. Document them alongside functional proof.

---


## 5) Worklog phase (snapshot + log in one file; required)

`WORKLOG.md` is the single handover artifact — and the Owner's dashboard. It combines a bounded current-state snapshot with an append-only log.

### Structure

`WORKLOG.md` has two regions:

1. **Snapshot sections** (top) — rewritten each chunk to reflect current state
2. **Log section** (bottom) — append-only dated entries

### Snapshot sections (rewrite each chunk)

Snapshot sections are organized into two clusters:

**Dashboard cluster (first screenful — "where are we, what's next, what's blocked"):**

- Goal (current interpretation of `SEED.md`)
- Current status (done / in progress / not started)
- Next actions (concrete, ordered)
- Open questions (risks, unknowns, research needs, hardening items — see below)

**Detail cluster (reference material — "how, why, what exactly"):**

- Plan (short sequence)
- Tasks (numbered; keep numbering stable where possible; dependency annotations allowed)
- Proof (pointer to `README.md` + environment notes + last known result)
- Decisions made (and why)
- Lessons (hard-won knowledge; append-only — see below)
- Practices (effective patterns; append-only — see below)
- Working set (files touched, branch/commit if applicable)
- Coordination (if external tool is in use: tool name + project location or key link pattern)

If SEED includes explicit compatibility targets (platforms/browsers/versions):

- Include a **Compatibility check status** section stating what has been verified and what remains.

### Proof steps: canonical location

`README.md` is the canonical and only location for proof steps. `WORKLOG.md` `## Proof` is a pointer — it references `README.md` and records environment notes (e.g., local quirks, runtime workarounds) and the last known result. Do not duplicate proof steps in `WORKLOG.md`.

### Lessons section (append-only within snapshot)

Unlike other snapshot sections, `## Lessons` is not rewritten each chunk. Entries are added at time of discovery and preserved across rewrites.

Each entry is a single concise line:

- what was learned
- why it matters (or what to avoid)

When the list exceeds ~15 entries, move the least-relevant ones to the top of `WORKLOG_ARCHIVE.md` under `## Archived lessons`. Note the move in the current log entry.

What goes in `## Lessons`:

- Failed approaches and why they failed
- Non-obvious system/API/environment behaviors
- Gotchas that cost time
- Anything an agent would want to tell the next agent: "don't repeat my mistake"

What doesn't go in (already has a home):

- Architectural choices → `## Decisions`
- Process preferences → `SEED.md` → `### Process knobs`
- Recurring mistakes worth enforcing → `SEED.md` → `## Guardrails` (see §11 → Promotion)
- Effective patterns worth repeating → `## Practices`
- Current open loops → `## Open questions`

### Practices section (append-only within snapshot)

Like `## Lessons`, `## Practices` is append-only — entries are added at time of discovery and preserved across rewrites. Where Lessons capture what went wrong, Practices capture what went right.

Each entry is a single concise line:

- what was done
- why it worked well (or what it enabled)

When the list exceeds ~15 entries, move the least-relevant ones to `WORKLOG_ARCHIVE.md` under `## Archived practices`. Note the move in the current log entry.

What goes in `## Practices`:

- Approaches that proved effective and are worth repeating
- Patterns that solved a class of problems, not just one instance
- Techniques that improved speed, quality, or clarity
- Anything an agent would want to tell the next agent: "do this again"

What doesn't go in (already has a home):

- Architectural choices → `## Decisions`
- Process preferences → `SEED.md` → `### Process knobs`
- Patterns worth standardizing across projects → promote to project skill (see §11a)
- Mistakes and gotchas → `## Lessons`

### Open questions section (replaces Risks / unknowns)

Captures anything unresolved: risks, unknowns, research needs, hardening items, things to verify before release.

Each entry has a type tag and status:

- Type tags: `[risk]`, `[question]`, `[hardening]`, `[verify]`
- Statuses: `OPEN`, `RESEARCHING` (with pointer to scratch doc if applicable), `RESOLVED`, `→ Task N`

Example:

```md
## Open questions

- [risk] Concurrent writes could corrupt state — no locking yet → OPEN
- [question] Which auth library fits offline constraint? → RESEARCHING, see docs/research-auth.md
- [hardening] Input validation missing on all endpoints → → Task 12
- [verify] Does Safari support the CSS grid we're using? → RESOLVED, yes (see Decisions)
```

When resolved, the conclusion feeds into its destination:

- `## Decisions` (if a choice was made)
- `## Lessons` (if something was learned)
- `## Tasks` (if work was identified)
- Or marked `RESOLVED` with a one-line answer inline

Resolved entries may be pruned during snapshot rewrites — the resolution lives in its destination.

### Log section (append each chunk)

Each chunk/session must append one entry to `## Log` including:

- date (YYYY-MM-DD), agent role and session name (e.g., `builder / StarNova`), and short summary
- descriptive hash tags
- what changed (high level)
- what proof was run / observed
- what's next
- owner feedback and actions taken (if checkpoint occurred)

**Lightweight format (non-behavioral chunks):**

Chunks with no behavioral changes (housekeeping, doc fixes, archive moves, formatting) may use a condensed format:

```md
### YYYY-MM-DD — <short title> (<role> / <session name>)

Housekeeping only. No behavioral changes. <what was done>. Proof: unchanged, still passing.
```

Use the full template for any chunk that alters observable output or requires new proof.

### Archive trigger

When the log exceeds ~10 entries, move older entries to `WORKLOG_ARCHIVE.md`, keeping the most recent 5 entries in `WORKLOG.md`. Note the archive cutoff date at the top of `## Log`.

---


## 5a) Housekeeping (idle time)

When implementation cannot proceed — waiting for owner feedback, blocked on a dependency, waiting for CI, or direction is unclear — use the time for housekeeping.

### Edit categories

- **Housekeeping-safe (allowed during idle):** doc drift fixes, formatting, archive moves, clarifying existing proof steps without changing behavior, stale reference cleanup, comment/typo fixes.
- **Behavioral (not allowed during idle):** code changes, dependency changes, new or modified proof, architectural changes — anything that alters observable output or requires new proof.

### Allowed during idle

In priority order:

1. **Freshen `WORKLOG.md`** — ensure snapshot sections reflect current state; catch any missed log entries.
2. **Run proof** — verify existing proof still passes; record result in `## Proof` → `### Last known result`.
3. **Check guardrails** — scan for violations of default (§11) and project-specific (`SEED.md` → `## Guardrails`) guardrails; fix or flag.
4. **Check archive triggers** — if log exceeds ~10 entries or lessons/practices exceed ~15, archive now.
5. **Audit `README.md`** — verify run/prove instructions match current behavior; fix drift (housekeeping-safe only).
6. **Self-audit** — run the playbook audit protocol (§7) and note any deviations in `## Open questions` or address them directly if they are housekeeping-safe fixes (e.g., missing log entry, stale working set).
7. **Review `## Lessons`** — check for relevance; consolidate duplicates; consider whether any should be promoted to guardrails.
8. **Review `## Practices`** — check for relevance; consolidate duplicates; consider whether any should be promoted to the project skill (see §11a).
9. **Review `## Open questions`** — check for stale entries; prune resolved items; flag items that need owner input.
10. **Clean working tree** — remove dead files, fix stale gitignore entries, ensure no secrets are tracked.

### Not allowed during idle

- Starting new tasks or features
- Behavioral edits (see categories above)
- Adding dependencies
- Expanding scope
- Making architectural changes

**Idle housekeeping must not produce behavioral changes that haven't been planned and claimed.** If housekeeping reveals work that should be done, add it to `## Tasks` — don't do it.

---


## 5b) Scratch docs (working documents)

For exploration that doesn't fit in a single `## Open questions` entry, create a working document.

**Location:** `docs/`

**Naming:** `docs/<type>-<topic>.md` where type is one of:

- `spike` — technical exploration / proof of concept
- `research` — comparison, market research, options analysis
- `design` — design alternatives, architecture options

**Lifecycle:**

1. Create when an open question needs extended exploration.
2. Reference from `## Open questions` while active.
3. Track in `## Working set` while active.
4. On resolution: conclusion feeds into `## Decisions`, `## Lessons`, or `## Tasks`.
5. Keep the doc as reference or delete it. Do not accumulate stale scratch docs.

**Scratch docs are supporting material. The conclusion must land in `WORKLOG.md`.**

Release records (`docs/releases/release-<id>.md`) are **not** scratch docs. They are permanent artifacts with their own lifecycle — see §10a.

---


## 6) Adapt phase (owner checkpoints)

At natural boundaries (milestone completion, direction uncertainty, scope edge), pause and present the Owner with a review guide.

Do not ask the Owner for numeric scores, ratings, or satisfaction metrics. The review guide and the Owner's own words are the feedback mechanism.

### 6.1 Approvals and exceptions

**Approval format:** Owner approvals must be explicit in writing (chat message, issue comment, or PR review). Examples: "Approved," "Yes — bundle A/B/C," "Go ahead with X." Silence or ambiguity is not approval. If unsure whether approval was given, ask.

**One-time exceptions:** When the Owner approves a one-time exception (merge without proof, temporary guardrail violation, scope deviation), record it in `WORKLOG.md` → `## Decisions` with the exception scope and reference it in the relevant log entry. Exceptions do not modify `SEED.md` — they are point-in-time waivers. If an exception effectively changes Vision, Constraints, or Non-goals (even temporarily), treat it as a `SEED.md` amendment (§0), not a one-time exception.

### Review guide (required at each checkpoint)

Address participants by name (from `## Participants`), not by role label.

Include in the checkpoint message:

1. **How to see it:** exact commands, URLs, or steps to observe the current state. Assume the Owner hasn't run anything since the last checkpoint. Include prerequisites if any.
2. **What to look at:** specific things to try, read, or poke at. Point to concrete files, screens, or outputs — not abstract dimensions.
3. **Current state vs. done:** what works now, what's not yet built, what's rough. Frame relative to SEED Vision so the Owner can gauge progress.
4. **What you want feedback on:** specific open questions where Owner input would change the next chunk. Prioritize decisions that are cheap to change now and expensive later.
5. **Guardrail proposals (if any):** lessons that should be promoted to guardrails (see §11 → Promotion).
6. **Practice promotions (if any):** practices that have proven effective across multiple chunks and should be promoted to the project skill (see §11a).

### Acting on feedback

If the Owner provides feedback:

- Actionable friction → address in the next chunk
- Process preference → record in `SEED.md` → `### Process knobs`
- Scope change → follow SEED amendment process (§0)
- New guardrail → add to `SEED.md` → `## Guardrails` with source tag (see §11)
- New skill practice → add to project skill file (see §11a)
- "Looks good, continue" → continue

Note feedback and resulting actions in the current `WORKLOG.md` log entry.

### Persist approved adaptations

If the Owner approves a process change:

- record it in `SEED.md` → `## Clarifications` → `### Process knobs`
- mention it in the latest `WORKLOG.md` log entry (so it's auditable)

### Change attribution rule

In Adapt iterations, prefer **one major change** per iteration to preserve attribution, unless the Owner explicitly approves bundling.

Bundling multiple major changes is allowed only if:

- the agent lists the exact bundled items before implementing, and
- the Owner explicitly approves the bundle (e.g., "yes, bundle improvements").

---


## 7) Playbook audit (optional, trigger-based)

The Owner (or a Reviewer agent) may request a "SEED Method audit" at any time. Recommended triggers:

- after Bootstrap
- before a major polish/bundle
- before declaring "done" / release
- when confusion arises about scope, proof, or handover quality

### Audit protocol

1. Read `AGENTS.md` (this document) as source of truth.
2. Inspect repo artifacts (at minimum: `SEED.md`, `WORKLOG.md`, `README.md`, deliverables, proof artifacts).
3. **Phase compliance** — report PASS / PARTIAL / FAIL for each phase:
   - SEED
   - Bootstrap
   - Build
   - Prove
   - Worklog
   - Adapt
4. **State clarity** — report CLEAR / UNCLEAR for each question:
   - **What's done?** Can you tell from `## Status` exactly what's been completed?
   - **What's next?** Do `## Next actions` (immediately below Status) reflect actual next steps, not stale leftovers?
   - **What's blocked?** Are all blockers visible in `## Open questions` with current statuses?
   - **What's in progress?** Is it obvious from the dashboard cluster what's actively being worked on right now, or that nothing is?
   - **What does the Owner need to do?** Is it clear whether the Owner needs to act (feedback, decision, approval) or whether agents can proceed autonomously?
5. **Guardrail compliance** — report PASS / VIOLATION for each guardrail:
   - Check every default guardrail (§11).
   - Check every project-specific guardrail (`SEED.md` → `## Guardrails`).
   - For each violation: state the guardrail, the evidence, and proposed fix.
6. **Learning system health** — report HEALTHY / STALE / GAPS for:
   - **Lessons:** are they being captured? Are any stale or redundant? Any candidates for guardrail promotion?
   - **Practices:** are they being captured? Are any stale or redundant? Any candidates for skill promotion?
7. Provide concrete evidence (file names + headings/bullets) for each finding.
8. List deviations and propose minimal fixes.

### Audit constraint

- **Housekeeping-safe fixes** (per §5a edit categories) discovered during the audit may be applied immediately. Record each fix inline with the relevant finding.
- **Behavioral fixes** must not be implemented during the audit. List them as proposed fixes and ask the Owner whether to apply them.

---


## 8) Default question script (when requesting SEED)

Ask any SEED questions not already answered. If the Owner has provided partial answers, confirm understanding and ask only the remaining questions.

Questions (in order):

1. Vision: what should exist when done?
2. Success criteria: what does a great result look like? What matters most?
3. Constraints: any hard requirements?
4. Non-goals: anything explicitly out of scope?
5. Stop conditions: when should we pause and ask you?
6. Timeline or urgency: any deadline or pace expectation? (affects ceremony level)

---


## 9) Multi-agent coordination (parallel execution contract)

When more than one agent is active, apply this section in addition to all global rules.

### External coordination tools (source of truth)

When an external coordination mechanism is in use (e.g., pi-messenger, OpenClawn), **it is the source of truth** for task claiming, file reservation, and agent communication. Do not duplicate coordination state in `WORKLOG.md` — reference the external system instead. The principles below (single ownership, escalation order, integration gate) still apply regardless of mechanism.

`WORKLOG.md` `## Tasks` remains required, but may be a thin index (task titles + external IDs/links). Ownership, claiming, and detailed status live in the external tool. The `## Coordination` line in `WORKLOG.md` snapshot sections must identify the tool and provide a link or location so the Owner can find the live state.

When no external tool is available: track ownership, reservations, and handoffs in `WORKLOG.md`. If git is also unavailable, treat `WORKLOG.md` task state + working set as the source of truth for branch-equivalent isolation and integration.

### 9.1 Task claiming and ownership

- Every active task has exactly one **owner agent** (identified by role label, not session name).
- Before implementation, owner claims task via the active coordination mechanism (external tool or `WORKLOG.md` `## Tasks`) with:
  - task id/title
  - owner (role label)
  - expected files/modules
- Only owner may close the task; reassignment must be recorded in `## Decisions`.

### 9.1a Shared artifact ownership

Unless explicitly delegated, the **driver/coordinator agent** owns updates to `WORKLOG.md`. Contributing agents report completion deltas through the active coordination mechanism (message, PR, handoff note — see §9.8). The coordinator integrates handoff notes into `WORKLOG.md` before merge/close and before Owner checkpoint messages. This prevents duplicate edits and race conditions on the shared file.

### 9.2 File/module reservation

- Reserve high-conflict paths before edits (module/feature folder preferred over single files when appropriate).
- If reservation overlaps:
  - earlier reservation has priority,
  - later agent either waits or negotiates scope split,
  - unresolved overlap is escalated (see 9.5).
- Keep reservations short-lived; release immediately after merge/handoff.

### 9.3 Branching and merge policy

Default: **trunk-based with short-lived task branches**.

- Branch naming: `task/<id>-<slug>` (or Owner-approved equivalent)
- Rebase/merge main frequently (at least daily for active branches)
- Prefer squash merge unless project policy requires otherwise
- No direct merge to main without passing integration gate (9.7)

### 9.4 Parallel work boundaries

- Prefer parallelization by independent modules/interfaces.
- For shared interfaces, agree contract first and record in `## Decisions`.
- If contract changes, notify affected task owners immediately and update plan/tasks.

### 9.5 Conflict resolution and decision escalation

Resolve in this order:

1. task owners attempt local resolution and document outcome,
2. if unresolved, coordinator/reviewer agent proposes minimal decision,
3. if still unresolved or user-facing/security/scope-impacting, escalate to Owner.

Escalate immediately for:

- SEED constraint conflicts
- irreversible architecture choices with broad impact
- security/privacy/network/persistence uncertainty
- scope changes beyond Vision

### 9.6 Parallel test/proof runs (non-interference rule)

Do not run tests or proof commands concurrently unless the project explicitly supports parallel test execution (e.g., isolated containers, per-agent ports, non-overlapping resources). If unsure, coordinate test runs sequentially:

- announce intent before running proof,
- wait for any in-progress proof run to complete,
- record your run only after exclusive access is confirmed.

If the proof slot is blocked beyond a reasonable wait window, escalate per §9.5.

### 9.7 Integration gate (required before merge/task close)

A task may be merged/closed only when:

- task acceptance criteria are met,
- required proof passes for changed behavior,
- branch is up to date with target branch (or equivalent integration baseline is confirmed by the active coordination mechanism when git/branches are unavailable),
- no unresolved reservation conflicts remain,
- `WORKLOG.md` is fresh and reflects the latest state/results.

Record gate result in the active coordination record and `WORKLOG.md`.

### 9.8 Handoff note template

Contributing agents (non-coordinator) use this template when reporting completed work via the coordination mechanism.

```md
### Handoff note — YYYY-MM-DD — <role> / <session name> · <model> — Task <N>

- Scope worked:
- Files touched:
- Behavior change summary:
- Proof run (commands + observed result):
- Open questions / blockers:
- Suggested next step:
```

Handoff notes are transient. The coordinator's integration into `WORKLOG.md` (snapshot sections + log entry) is the durable record. Handoff notes do not need to be individually referenced after integration.

---


## 10) Definition of Release Done (DoRD)

Use this checklist before declaring "done" / requesting Owner sign-off.

- [ ] **Vision delivered:** implementation matches `SEED.md` Vision and respects Constraints/Non-goals.
- [ ] **Tasks closed:** all planned tasks are completed, explicitly deferred, or explicitly dropped with reason.
- [ ] **Final proof passed:** required automated/manual proof executed for the full product; results recorded.
- [ ] **Guardrails clean:** no violations of default (§11) or project-specific guardrails.
- [ ] **No critical unknowns:** open questions are accepted by Owner or tracked as post-release items.
- [ ] **Learning captured:** `## Lessons` and `## Practices` are current; no unrecorded insights from the release.
- [ ] **Artifacts fresh:**
  - [ ] `WORKLOG.md` is final and fresh
  - [ ] `SEED.md` clarifications/process knobs/guardrails are up to date
- [ ] **README current:** run/prove instructions and known limitations match shipped behavior.
- [ ] **Working tree clean enough to hand over:** no ambiguous partial edits; rollback path is clear.
- [ ] **Owner sign-off requested:** explicit "ready for release" handoff sent.
- [ ] **(Optional) Release metadata:** version/tag/changelog created if repo workflow uses releases.

### Release readiness summary (optional, pre-sign-off)

```md
Release readiness: PASS
Date: YYYY-MM-DD
Scope: <what is included>
Proof summary: <commands/checklist + outcome>
Guardrail check: PASS / <list violations>
Deferred items: <none or list>
Owner sign-off: <requested/received>
```

After Owner sign-off, proceed to §10a.

---


## 10a) Post-release closeout (seal + reset)

After the Owner signs off on a release, perform closeout in two steps. The goal is to seal the completed release as an immutable record, then reset the working surface so the next release can start cleanly without losing auditability.

In multi-agent mode, the coordinator performs both steps. Contributing agents do not modify `WORKLOG.md`, create release records, or perform archive/reset directly.

### Release ID scheme

Default: **SemVer** (`vX.Y.Z`). If the project uses a different scheme (date-based, incremental, etc.), record the convention in `SEED.md` → `### Process knobs`.

### Step A — Seal (create the release marker)

1. **Tag the commit** (preferred): `vX.Y.Z` on the exact release commit.
2. **Create the release record:** `docs/releases/release-vX.Y.Z.md`

The release record is the canonical, self-contained artifact for this release. It must include:

```md
# Release vX.Y.Z

- **Date:** YYYY-MM-DD
- **Tag / commit:** <tag or hash>

## Scope

- Included:
- Excluded / deferred:

## Proof summary

(Self-contained — readable without opening WORKLOG_ARCHIVE.md or CI.)

- Commands / checklist:
- Result (pass/fail):
- CI run link (if available):

## Exceptions

(One-time exceptions granted during this release, if any. Reference WORKLOG decisions.)

- <none or list>

## Known limitations / follow-ups

- ...

## References

(PRs, issues, coordination threads, external links — if applicable.)

- ...
```

Release records are **permanent artifacts**, not scratch docs. They are not subject to §5b's stale-doc cleanup rule.

### Step B — Archive + reset

**Archive** (preserve full history):

1. Move **all remaining log entries** for this release from `WORKLOG.md` → `## Log` into `WORKLOG_ARCHIVE.md` under a header: `## Release vX.Y.Z (YYYY-MM-DD)`. Do not curate or select — archive everything.
2. Add an entry to the `## Releases` index at the top of `WORKLOG_ARCHIVE.md`:
   - release id, date, and link to the release record (`docs/releases/release-vX.Y.Z.md`).

**Reset** (prepare for next release):

Reset each `WORKLOG.md` snapshot section for the new working surface:

| Section             | Reset behavior                                                                                                                                                                |
| ------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `## Goal`           | Unchanged (still from `SEED.md`; update if SEED is amended for the next release)                                                                                              |
| `## Status`         | "Released vX.Y.Z on YYYY-MM-DD. Next release: planning / not started."                                                                                                        |
| `## Next actions`   | Fresh list for next release (e.g., "Owner: confirm scope for next release", "Agent: create tasks for vX.Y.(Z+1)").                                                            |
| `## Open questions` | Prune resolved entries. Carry forward still-open items, labeled `[carry-over from vX.Y.Z]`.                                                                                   |
| `## Plan`           | Clear completed items. Seed with next-release planning steps if known.                                                                                                        |
| `## Tasks`          | Remove completed tasks. Carry forward deferred tasks, explicitly labeled `[carry-over from vX.Y.Z]`.                                                                          |
| `## Proof`          | Reset `### Last known result` to blank. Pointer to `README.md` remains unless the proof mechanism itself is changing.                                                         |
| `## Decisions`      | Archive release-specific decisions into the release record (Step A). Carry forward architectural and cross-cutting decisions that affect future work, labeled `[carry-over]`. |
| `## Lessons`        | **Persist.** Lessons are institutional memory and survive across releases. Continue normal append-only / archive-at-15 behavior.                                              |
| `## Practices`      | **Persist.** Practices are institutional memory and survive across releases. Continue normal append-only / archive-at-15 behavior.                                            |
| `## Working set`    | Clear.                                                                                                                                                                        |
| `## Coordination`   | Unchanged (if external tool is still in use).                                                                                                                                 |
| `## Log`            | Note archive cutoff: "Entries through vX.Y.Z archived to WORKLOG_ARCHIVE.md." Start fresh.                                                                                    |

After reset, `WORKLOG.md` should be a clean, lightweight dashboard ready for the next cycle.

### SEED across releases

`SEED.md` describes the **project**, not a single release. It persists across releases and is not reset or recreated during closeout.

- If the next release changes Vision, Constraints, or Non-goals → use the SEED amendment process (§0).
- If it doesn't → no SEED change needed. New scope lives in `WORKLOG.md` → `## Plan` / `## Tasks`.

**SEED consolidation (optional):** when accumulated amendments make `SEED.md` hard to read, the Owner may approve a consolidation — rewriting `SEED.md` to reflect current project reality. When consolidating:

1. Archive the pre-consolidation `SEED.md` text into the most recent release record (`docs/releases/release-<id>.md`) under `## Pre-consolidation SEED`.
2. Rewrite `SEED.md` incorporating all prior amendments. Preserve the current `## Guardrails` and `### Process knobs` sections as-is (they are already current).
3. Note the consolidation in `## Amendments`: "Consolidated through vX.Y.Z — YYYY-MM-DD."

Consolidation requires Owner approval. The rewritten SEED becomes the new baseline.

---


## 11) Guardrails (enforced rules that evolve)

Guardrails are rules that are always checked — at every chunk (DoD), every audit, and every release. Unlike lessons (advisory) or constraints (static), guardrails accumulate over the life of a project.

### Three sources

| Source            | Where recorded              | Who adds                                                           |
| ----------------- | --------------------------- | ------------------------------------------------------------------ |
| **Default**       | This playbook (below)       | Ship with playbook; always active unless SEED explicitly overrides |
| **Owner-imposed** | `SEED.md` → `## Guardrails` | Owner, at any time                                                 |
| **Learned**       | `SEED.md` → `## Guardrails` | Agent proposes, Owner approves                                     |

### Default guardrails

These are always active unless `SEED.md` explicitly overrides a specific guardrail.

1. **No secrets in git** — never commit credentials, API keys, tokens, or other sensitive data. Use `.env` + `.gitignore`. _(Non-negotiable — cannot be overridden.)_
2. **Proof passes before commit** — if proof exists, run it before committing when feasible. If proof cannot run due to infrastructure/tooling outage, commits may proceed on a branch, but must not merge to main or close tasks until proof is run and recorded. If no proof exists yet, the first commit must include at least an initial proof mechanism (even minimal). All subsequent commits must keep proof green.
3. **Single canonical source (avoid divergence)** — for any spec, config, or decision, designate one canonical location. Other files may summarize or reference it, but must not introduce new details that can drift. (`WORKLOG.md` `## Proof` is a pointer to `README.md`, not a copy — see §5.)
4. **Dependencies require justification** — every external dependency added must be recorded in `## Decisions` with rationale. Prefer fewer dependencies.
5. **No orphan artifacts** — every file in the repo must be referenced from `WORKLOG.md` (working set or open questions), `README.md`, or project source. Unreferenced files are deleted or referenced. Release records (`docs/releases/`) are referenced from the `## Releases` index in `WORKLOG_ARCHIVE.md`.
6. **Repo organization** — required artifacts (`SEED.md`, `README.md`, `WORKLOG.md`, `AGENTS.md`, and `WORKLOG_ARCHIVE.md` / `SEED_REQUEST.md` when present) in repo root. Scratch docs in `docs/`. Release records in `docs/releases/`. No loose files in root beyond required artifacts and standard project config (e.g., `package.json`, `Makefile`, `.gitignore`).
7. **No generated files in git** — do not commit build artifacts, derived files, or files generated from scratch unless SEED explicitly requires it. Lockfiles that ensure reproducible builds (e.g., `package-lock.json`) are allowed.
8. **Naming conventions** — project and source files: kebab-case. Scratch docs: `docs/<type>-<topic>.md`. Release records: `docs/releases/release-<id>.md`. Branches: `task/<id>-<slug>`. Playbook-mandated artifact names (`README.md`, `WORKLOG.md`, `WORKLOG_ARCHIVE.md`, `SEED.md`, `SEED_REQUEST.md`, `AGENTS.md`) are exempt. Follow language/ecosystem conventions when they conflict. Be consistent within the project.
9. **Derived data is computed, not hand-maintained** — values derivable from source (counts, statistics, generated indexes) should be produced by script or build step, not manually updated across files. When automation is impractical, designate one canonical location for the value; other files reference it. Never maintain the same derived value in multiple places by hand.

### Overriding a default guardrail

If a SEED requires overriding a default guardrail (except #1, which is non-negotiable):

- State the override explicitly in `SEED.md` → `## Guardrails` with rationale.
- The override applies only to the stated guardrail. All others remain active.

### Promotion: lesson → guardrail

When a lesson in `## Lessons` represents a mistake that is significant enough to enforce (cost real time, broke things, or is likely to recur):

1. Agent proposes the guardrail at the next owner checkpoint (§6), stating:
   - the lesson it comes from
   - the proposed guardrail wording (one line, actionable)
   - why advisory isn't enough (what goes wrong if it's ignored)
2. Owner approves or rejects.
3. If approved: add to `SEED.md` → `## Guardrails` with source tag `[learned]` and date.
4. The original lesson may remain in `## Lessons` or be pruned — the guardrail is now the enforced version.

### Project-specific guardrails in `SEED.md`

Each entry includes:

- The rule (one line, actionable)
- Source tag: `[owner]` or `[learned]`
- Date added

Example:

```md
## Guardrails

- No direct database queries outside the repository layer — all access through `db/` module. [owner] 2025-01-15
- Always validate webhook signatures before processing payload — skipping this caused a 2-hour debug session. [learned] 2025-02-01
- Maximum 3 external API calls per user-facing request — performance budget. [owner] 2025-01-15
```

---


## 11a) Practice promotion (best practices that evolve)

Practices are the positive counterpart to guardrails. Where guardrails prevent mistakes from recurring, practices ensure successes are repeated. Together they form the complete learning system:

```
Negative:  Mistake  → Lesson   → Guardrail  (project-enforced, prevents recurrence)
Positive:  Success  → Practice → Skill      (cross-project portable, enables reuse)
```

### Promotion: practice → skill

When a practice in `## Practices` has proven effective across multiple chunks, contexts, or projects, and represents reusable knowledge:

1. Agent proposes promotion at the next owner checkpoint (§6), stating:
   - the practice it comes from
   - why it's generalizable (not just situational)
   - proposed wording for the skill entry
2. Owner approves or rejects.
3. If approved: add to the project's skill file (e.g., `SKILL.md`) or to an organizational skill library, with source tag `[learned]` and date.
4. The original practice may remain in `## Practices` or be pruned — the skill entry is now the reusable version.

### What makes a practice promotable

Not every practice should become a skill. Promotion criteria:

- **Proven across contexts** — worked in more than one chunk, task type, or module. A pattern that worked once might be coincidence.
- **Generalizable** — applicable beyond this specific project. "Use Redis for caching" is project-specific; "define cache interface before choosing implementation" is generalizable.
- **Teachable** — can be explained in 1–2 lines clearly enough that a new agent can apply it without additional context.
- **Not already a process knob or guardrail** — process preferences go in `### Process knobs`; enforced rules go in `## Guardrails`. Skills are advisory best practices.

### Skill entry format

Each promoted practice in the skill file includes:

- The practice (one line, actionable)
- Source tag: `[learned]`
- Date promoted
- Origin (optional): project or context where it was discovered

Example:

```md
- Write API contract as a scratch doc before implementation — eliminates integration back-and-forth. [learned] 2025-03-15
- Run proof after every 2-3 file changes, not just per chunk — catches regressions 10x faster. [learned] 2025-04-01
- Split middleware into pure transform + side-effect layers — each independently testable. [learned] 2025-04-20
```

---


# Appendix — Skeletons (all referenced artifacts)

## A) `SEED.md` skeleton

```md
# SEED

## Participants

(Owner names are stable identifiers. Agent names are session-scoped and may change
on renewal — use role labels for cross-session traceability. Platform records the
typical orchestrator · model; actual per-session values go in log entries when they differ.)

| Role          | Name/Handle   | Platform                 | Notes |
| ------------- | ------------- | ------------------------ | ----- |
| Owner         |               | —                        |       |
| Agent: <role> | (per session) | <orchestrator> · <model> |       |

## Vision (Owner wording)

(1–3 sentences. What should exist when done?)

## Success criteria

(What does a great result look like? What matters most?)

- ...
- ...
- ...

## Constraints

(Tech choices, offline/no-network, dependency policy, single-file, platforms, perf budgets, etc.)

- Constraint:
- Constraint:

## Guardrails

(Enforced rules. Default guardrails from AGENTS.md §11 are always active. Add project-specific guardrails here. Tag each with source and date.)

(To override a default guardrail, state it explicitly here with rationale. Default #1 — no secrets in git — cannot be overridden.)

- Guardrail. [owner/learned] YYYY-MM-DD
- Guardrail. [owner/learned] YYYY-MM-DD

## Non-goals

(Explicitly out of scope.)

- Non-goal:
- Non-goal:

## Stop conditions

(When to pause and ask before proceeding.)

- Stop if:
- Stop if:

## Timeline / urgency

(Deadline, pace expectation, or "no deadline".)

- Timeline:

## Clarifications

(Only necessary clarifications; keep Owner's wording above intact.)

- Clarification:

### Process knobs

(Approved adaptations that become new defaults for this repo.)

- Knob:
  - Setting:
  - Why:
  - Since (YYYY-MM-DD):

## Amendments

(Owner-approved scope changes after initial SEED, with date and rationale. If consolidated, note: "Consolidated through vX.Y.Z — YYYY-MM-DD." See §10a.)

- Amendment (YYYY-MM-DD):
  - Change:
  - Rationale:
```

---

## B) `SEED_REQUEST.md` skeleton

```md
# SEED REQUEST

Owner input is required before implementation can begin.

## Questions (answer in order)

1. Vision: what should exist when done?
2. Success criteria: what does a great result look like? What matters most?
3. Constraints: any hard requirements?
4. Non-goals: anything explicitly out of scope?
5. Stop conditions: when should we pause and ask you?
6. Timeline or urgency: any deadline or pace expectation?

## Assumptions (UNAPPROVED)

(Only fill this if helpful; do not implement based on these.)

- Assumption:
- Assumption:
```

---

## C) `README.md` skeleton (minimal "run + prove")

```md
# <Project Name>

## What this is

(1–3 sentences aligned with `SEED.md` Vision.)

## How to run

- Prereqs:
- Install:
- Run:

## How to prove it works

(Prefer a single command; otherwise a short checklist.)

- Automated:
  - Command:
  - Expected:
- Manual (if needed):
  1.
  2.
  3.

## Notes

- Constraints:
- Known limitations:
```

---

## D) `WORKLOG.md` skeleton (snapshot + log; required)

```md
# WORKLOG

<!-- Dashboard cluster: first screenful — where are we, what's next, what's blocked -->

## Goal (from `SEED.md`)

- ...

## Status

- Done:
- In progress:
- Not started:

## Next actions

1.
2.
3.

## Open questions

(Type tags: [risk], [question], [hardening], [verify]. Statuses: OPEN, RESEARCHING, RESOLVED, → Task N. Prune resolved entries during snapshot rewrites.)

- ...

<!-- Detail cluster: reference material — how, why, what exactly -->

## Plan

1.
2.
3.

## Tasks

(Keep numbering stable where possible. Dependency annotations allowed, e.g., "blocked by Task 1".)

1. [ ] ...
2. [ ] ...
3. [ ] ...

## Proof

See [README.md → How to prove it works](README.md#how-to-prove-it-works)

### Environment notes

- (Local quirks, runtime workarounds, or agent-specific setup — if any)

### Last known result

- Date (YYYY-MM-DD):
- Result (local / CI link):
- Notes:

## Decisions

- Decision:
  - Why:
  - Decided by: <participant name>

## Lessons

(Append-only. One line per lesson. Do not remove during snapshot rewrites. Archive to `WORKLOG_ARCHIVE.md` when list exceeds ~15 entries. If a lesson is significant enough to enforce, propose promotion to guardrail — see §11.)

- ...

## Practices

(Append-only. One line per practice. Do not remove during snapshot rewrites. Archive to `WORKLOG_ARCHIVE.md` when list exceeds ~15 entries. If a practice is proven and generalizable, propose promotion to project skill — see §11a.)

- ...

## Working set

- Files touched:
- Branch:
- Commit:
- Environment notes (if relevant; e.g., tools installed, runtime workarounds):

## Coordination

(Only when external coordination tool is in use. Remove this section if not applicable.)

- Tool:
- Location / link:

---

## Log

(Append-only. Keep most recent ~5 entries here. When log exceeds ~10 entries, archive older entries to `WORKLOG_ARCHIVE.md` and note cutoff date below.)

### YYYY-MM-DD — <short title> (<role> / <session name> · <model if different from default>)

**Summary**

- ...

**Changes**

- ...

**Proof run**

- Command/steps:
- Expected:
- Observed:

**Decisions**

- Decision:
  - Why:
  - Decided by: <participant name>

**Next**

1.
2.
3.

**Feedback from <participant name> (if checkpoint occurred)**

- Feedback:
- Action taken:
```

---

## E) `WORKLOG_ARCHIVE.md` skeleton

```md
# WORKLOG ARCHIVE

## Releases

(Index of all release records. One line per release.)

| Release | Date       | Record                                                             |
| ------- | ---------- | ------------------------------------------------------------------ |
| vX.Y.Z  | YYYY-MM-DD | [docs/releases/release-vX.Y.Z.md](docs/releases/release-vX.Y.Z.md) |

---

## Archived lessons

(Overflow from WORKLOG.md ## Lessons when list exceeds ~15 entries.)

- ...

## Archived practices

(Overflow from WORKLOG.md ## Practices when list exceeds ~15 entries.)

- ...

---

(Archived log entries below, most recent release first.)

## Release vX.Y.Z (YYYY-MM-DD)

(Final snapshot sections and all log entries for this release, moved here during §10a closeout.)

### YYYY-MM-DD — <short title>

...
```

---

## F) Release record skeleton

```md
# Release vX.Y.Z

- **Date:** YYYY-MM-DD
- **Tag / commit:** <tag or hash>

## Scope

- Included:
- Excluded / deferred:

## Proof summary

(Self-contained — readable without opening WORKLOG_ARCHIVE.md or CI.)

- Commands / checklist:
- Result (pass/fail):
- CI run link (if available):

## Exceptions

(One-time exceptions granted during this release, if any. Reference WORKLOG decisions.)

- <none or list>

## Known limitations / follow-ups

- ...

## References

(PRs, issues, coordination threads, external links — if applicable.)

- ...

## Pre-consolidation SEED

(Only present if SEED was consolidated at this release. Full text of previous SEED.md preserved for audit trail.)
```

---

## G) `.env.example` skeleton (if needed)

```bash
# Copy to .env (do not commit .env)
# Add only non-secret defaults here; document secrets clearly.

# EXAMPLE_VAR=
```

---

## H) `.gitignore` snippet (minimum for secrets)

```gitignore
.env
.env.*
!.env.example
```
