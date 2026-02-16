# The SEED Method

```
 ░▒▓███████▓▒░▒▓████████▓▒░▒▓████████▓▒░▒▓███████▓▒░
░▒▓█▓▒░      ░▒▓█▓▒░      ░▒▓█▓▒░      ░▒▓█▓▒░░▒▓█▓▒░
░▒▓█▓▒░      ░▒▓█▓▒░      ░▒▓█▓▒░      ░▒▓█▓▒░░▒▓█▓▒░
 ░▒▓██████▓▒░░▒▓██████▓▒░ ░▒▓██████▓▒░ ░▒▓█▓▒░░▒▓█▓▒░
       ░▒▓█▓▒░▒▓█▓▒░      ░▒▓█▓▒░      ░▒▓█▓▒░░▒▓█▓▒░
       ░▒▓█▓▒░▒▓█▓▒░      ░▒▓█▓▒░      ░▒▓█▓▒░░▒▓█▓▒░
░▒▓███████▓▒░░▒▓████████▓▒░▒▓████████▓▒░▒▓███████▓▒░
```

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

### 1. Add the playbook to your repo

Choose one of these layouts.

#### Option A (recommended): token-efficient kernel + modular sections

This keeps `AGENTS.md` small (always loaded) and stores detailed guidance in
separate files that the agent reads only when needed.

```bash
# Kernel (small, always loaded)
cp dist/AGENTS.kernel.md /path/to/your/repo/AGENTS.md

# Modular section files (load on demand)
mkdir -p /path/to/your/repo/docs
cp -R docs/playbook /path/to/your/repo/docs/
```

#### Option B: single-file bundle

Copy the full playbook into a single `AGENTS.md`:

```bash
cp dist/AGENTS.md /path/to/your/repo/AGENTS.md
```

> **Note:** Some tools have a default limit on how many bytes they load from
> `AGENTS.md` (for example, Codex defaults to 32 KiB). If your tool truncates
> instructions, Option A avoids the issue. Otherwise, increase the limit (for
> Codex, set `project_doc_max_bytes` in `~/.codex/config.toml`).

### 2. Start your first session

Tell your agent:

> Load AGENTS.md and follow it as the root playbook.

The agent will ask you the SEED questions (Vision, Success criteria, Constraints, Non-goals, Stop conditions, Timeline) and write `SEED.md` — then Bootstrap begins automatically.

### 3. That's it

The method handles the rest: planning, building in verifiable chunks, proof,
worklog updates, and owner checkpoints. Read `WORKLOG.md` anytime to see exactly
where things stand.

## Use the playbook in popular coding agents

The SEED Method uses plain Markdown files, so you can use it with most coding
agents.

### pi

pi reads `AGENTS.md` from your repo root and loads it automatically.

1. Add the playbook to your repo (use Quick Start Option A for a small
   `AGENTS.md`).
2. Start `pi` in that repo.

### Codex (OpenAI)

Codex reads `AGENTS.md` as the project instruction file.

1. Add the playbook to your repo (use Quick Start Option A for a small
   `AGENTS.md`).
2. Start Codex in that repo.

If Codex truncates your instructions, increase the byte limit in
`~/.codex/config.toml`:

```toml
project_doc_max_bytes = 65536
```

Codex also supports nested `AGENTS.md` files in subdirectories for monorepos.

### Claude Code (Anthropic)

Claude Code loads `CLAUDE.md` by default. Use one of these options:

- Rename the file to `CLAUDE.md`.
- Keep `AGENTS.md` and create a small `CLAUDE.md` that imports it:

```md
@AGENTS.md
```

For modular rules, use `.claude/rules/*.md`.

### Gemini

Gemini environments commonly load `GEMINI.md` by default.

To keep `AGENTS.md` as the canonical playbook, create a `GEMINI.md` in your repo
root that imports it:

```md
@AGENTS.md
```

For Gemini CLI, you can also configure the accepted context filenames in
`.gemini/settings.json`:

```json
{ "context": { "fileName": ["AGENTS.md", "GEMINI.md"] } }
```

### OpenCode

OpenCode reads `AGENTS.md` for project rules.

1. Add the playbook to your repo (use Quick Start Option A for a small
   `AGENTS.md`).
2. Start OpenCode in that repo.

To keep `AGENTS.md` compact and load deeper docs, add extra instruction files in
`opencode.json`:

```json
{
  "$schema": "https://opencode.ai/config.json",
  "instructions": ["docs/seed-method.md"]
}
```

## Use the extended playbook as a skill (keep `AGENTS.md` compact)

Some tools load a limited number of bytes from `AGENTS.md`. To keep your always
loaded instructions small, split your guidance:

1. Keep a short `AGENTS.md` in your repo root (kernel rules + pointers).
2. Store the full SEED Method text in a separate file (for example
   `docs/seed-method.md`) or as modular sections.
3. Load that extended guidance only when needed using your tool's skill or
   rules mechanism.

If you use the open agent skills ecosystem, you can install the SEED Method as
an on-demand skill from this repo. Learn more at https://skills.sh/.

```bash
npx skills add jveres/The-SEED-Method@seed
```

Skill page: https://skills.sh/jveres/The-SEED-Method/seed

This installs `skills/seed/` into your agent's skills directory. The full
playbook text is in `skills/seed/PLAYBOOK.md`.

Examples:

- **Claude Code:** Put detailed, topic-specific rules into `.claude/rules/*.md`,
  or use `@` imports from `CLAUDE.md`.
- **Gemini CLI:** Use `GEMINI.md` imports and module-specific context files in
  subdirectories.
- **OpenCode:** Prefer `opencode.json` `instructions` to include additional
  files, and keep `AGENTS.md` short.
- **pi:** Put the extended guidance in a skill (for example
  `~/.pi/agent/skills/seed/SKILL.md`) and keep your repo `AGENTS.md` limited to
  gating rules plus “use the skill when process guidance is needed.”

## What's in this repo

These are the canonical sources and the generated artifacts that you copy into
other repos.

| File                                                 | Description                                                                    |
| ---------------------------------------------------- | ------------------------------------------------------------------------------ |
| [`dist/AGENTS.kernel.md`](dist/AGENTS.kernel.md)     | Token-efficient kernel to copy into your repo as `AGENTS.md`                   |
| [`dist/AGENTS.md`](dist/AGENTS.md)                   | Single-file playbook build to copy into your repo as `AGENTS.md`               |
| [`scripts/build-dist.py`](scripts/build-dist.py)     | Builds dist variants (`--variant kernel` builds `dist/AGENTS.kernel.md`)       |
| [`scripts/check-dist.py`](scripts/check-dist.py)     | Verifies `dist/AGENTS.md` and `dist/AGENTS.kernel.md` are up to date           |
| [`scripts/build-skill.py`](scripts/build-skill.py)   | Builds the skills.sh skill files from `dist/AGENTS.md`                         |
| [`scripts/check-skill.py`](scripts/check-skill.py)   | Verifies the skills.sh skill files are up to date                              |
| [`skills/seed/`](skills/seed)                        | skills.sh skill: `SKILL.md` (entrypoint) + `PLAYBOOK.md` (full playbook text)  |
| [`CLAUDE.md`](CLAUDE.md)                             | Claude Code shim — imports `AGENTS.md`                                         |
| [`GEMINI.md`](GEMINI.md)                             | Gemini CLI shim — imports `AGENTS.md`                                          |
| [`SEED-Method.md`](SEED-Method.md)                   | Kernel + index for this source repo (canonical content is in `docs/playbook/`) |
| [`docs/playbook/`](docs/playbook)                    | Canonical modular playbook section sources                                     |
| [`SEED-Method-Cookbook.md`](SEED-Method-Cookbook.md) | Practical patterns for adapting the method to real-world situations            |
| [`CHANGELOG.md`](CHANGELOG.md)                       | Version history and cumulative changes                                         |
| [`LICENSE`](LICENSE)                                 | CC BY 4.0                                                                      |

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

- **`AGENTS.md`** — The playbook itself
  - Token-efficient: copy `dist/AGENTS.kernel.md`
  - Single-file bundle: copy `dist/AGENTS.md`
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

Current version: **3.5.1**

See [`CHANGELOG.md`](CHANGELOG.md) for the full version history.

## License

This work is licensed under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) — you're free to use, adapt, and share it with attribution.
