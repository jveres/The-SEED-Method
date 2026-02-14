# The SEED Method

**A development methodology for agent-driven projects.**
One human. Any number of agents. The method keeps it manageable.

## What is SEED?

The SEED Method is a lightweight, structured playbook for projects where a single human Owner directs one or more AI agents. It defines clear phases, artifacts, coordination patterns, and guardrails that keep agent-driven work predictable, auditable, and high-quality.

**Core flow:** SEED → Bootstrap → Build → Prove → Worklog → Adapt

**Eight principles:**

1. SEED gates everything — no work without direction
2. Proof over opinion — observable evidence is the only currency
3. One canonical source — every fact has one home
4. The worklog is the dashboard — current state is always readable
5. Guardrails accumulate — learn from mistakes, then lock them in
6. Owner decides, agents execute — authority is clear
7. Stop when stuck — don't guess past a blocker
8. Learn both ways — capture what went wrong _and_ what went right

## Quick Start

### 1. Copy the playbook into your repo

Copy [`SEED-Method.md`](SEED-Method.md) into your repo root and rename it to `AGENTS.md`:

```bash
cp SEED-Method.md /path/to/your/repo/AGENTS.md
```

### 2. Start your first session

Tell your agent:

> Load AGENTS.md and follow it as the root playbook.

The agent will ask you the SEED questions (Vision, Success criteria, Constraints, Non-goals, Stop conditions, Timeline) and write `SEED.md` — then Bootstrap begins automatically.

### 3. That's it

The method handles the rest: planning, building in verifiable chunks, proof, worklog updates, and owner checkpoints. Read `WORKLOG.md` anytime to see exactly where things stand.

## What's in this repo

| File                                                 | Description                                                                     |
| ---------------------------------------------------- | ------------------------------------------------------------------------------- |
| [`SEED-Method.md`](SEED-Method.md)                   | The complete SEED Method playbook (v3.5) — this is what you copy into your repo |
| [`SEED-Method-Cookbook.md`](SEED-Method-Cookbook.md) | Practical patterns for adapting the method to real-world situations             |
| [`CHANGELOG.md`](CHANGELOG.md)                       | Version history and cumulative changes                                          |
| [`LICENSE`](LICENSE)                                 | CC BY 4.0                                                                       |

## How it works

```
Owner provides SEED (vision, constraints, criteria)
        │
        ▼
┌─────────────────┐
│    Bootstrap    │  Set up project, plan, proof mechanism, WORKLOG.md
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│      Build      │  Work in verifiable, revertible chunks
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│      Prove      │  Every meaningful change has evidence
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│     Worklog     │  Snapshot + log — the Owner's dashboard
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│      Adapt      │  Owner checkpoints, feedback, guardrails evolve
└────────┬────────┘
         │
         ▼
      Release        Owner sign-off, seal, archive, reset
```

## Key artifacts

When you use the SEED Method in a project, it creates and maintains these files:

- **`AGENTS.md`** — The playbook itself (copy of `SEED-Method.md`)
- **`SEED.md`** — Owner's vision, constraints, and project-specific guardrails
- **`WORKLOG.md`** — Living dashboard: status, plan, tasks, proof, decisions, lessons
- **`README.md`** — How to run and prove the project works

## Multi-agent support

The SEED Method supports parallel execution with multiple agents (§9). It defines:

- Task claiming and single ownership
- File/module reservation to prevent conflicts
- Branching and merge policy
- Integration gates before merge
- Handoff note templates for clean coordination

Works with external coordination tools (e.g., pi-messenger) or standalone via `WORKLOG.md`.

## The Cookbook

The [SEED Method Cookbook](SEED-Method-Cookbook.md) covers real-world adaptation patterns:

- Monorepos, retrofitting existing codebases, tiny one-shot projects
- Long-running projects with many releases
- Working without git, UI/visual projects, libraries vs. applications
- Spikes and research, CI integration, and more

## Version

Current version: **3.5**

See [`CHANGELOG.md`](CHANGELOG.md) for the full version history.

## License

This work is licensed under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) — you're free to use, adapt, and share it with attribution.
