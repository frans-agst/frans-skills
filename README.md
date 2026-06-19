# frans-skills

A personal AI-engineering workflow that combines three skill sets into one loop centered on **`/grill-with-docs`**, with persistent **progress tracking** so you always know where the build stands.

- Depth from [mattpocock/skills](https://github.com/mattpocock/skills)
- Continuity & discipline from [jsm-pro-skills](https://github.com/frans-agst/jsm-pro-skills)
- A `context/` substrate from [context-driven-dev](https://github.com/frans-agst/context-driven-dev)

👉 **Read [WORKFLOW.md](./WORKFLOW.md) for the actual workflow.** This file is just setup.

**The loop at a glance:**

```
/remember restore → /grill-with-docs → /to-plan → /design-system (UI features)
   → /implement (/tdd, /codebase-design) → /imprint → /review → /design-review (UI features)
   → /verify → update progress-tracker → /remember save
```

Breakage: `/recover` → `/diagnosing-bugs`. Upkeep: `/improve-codebase-architecture`, `/imprint audit`.

> **Stack assumption:** the UI templates and the `/verify` step assume a **Next.js + Tailwind v4 + shadcn** project (shadcn for the itshover icon registry), and `/design-system` needs the **ui-ux-pro-max** engine installed (see Setup). The engineering skills (grill, plan, tdd, review, etc.) are stack-agnostic — only the UI templates, the icon source, and the dev-server run command bake the stack in. Swap those if your stack differs.

---

## What's here

```
frans-skills/
├── WORKFLOW.md             # the workflow + cheat-sheet (start here)
├── skills/                 # the combined, adapted skill set
├── templates/
│   ├── context/            # copy into each project as /context
│   └── CLAUDE.md           # ritual block to merge into a project's CLAUDE.md
├── context-driven/         # original source clone (reference — safe to delete)
└── jsm-pro/                # original source clone (reference — safe to delete)
```

### Skills

| Skill | From | Role |
|---|---|---|
| `grill-with-docs` | mattpocock | Relentless alignment; writes `context/glossary.md` + ADRs |
| `grilling`, `domain-modeling` | mattpocock | The engines behind `grill-with-docs` |
| `to-plan` | frans (new) | Synthesize the grilling into a `build-plan.md` feature entry — the no-issues analog of `/to-prd` |
| `design-system` | frans (new) | **Design-creation step.** Runs [ui-ux-pro-max](https://github.com/nextlevelbuilder/ui-ux-pro-max-skill) to generate the design → `context/design-system.md`, syncs `ui-tokens.md` / `ui-rules.md` |
| `implement` | mattpocock | Build the current build-plan feature |
| `tdd`, `codebase-design` | mattpocock | Red-green-refactor + deep-module design |
| `diagnosing-bugs` | mattpocock | Rigorous, feedback-loop-first bug diagnosis |
| `improve-codebase-architecture` | mattpocock | Periodic ball-of-mud rescue |
| `remember` | jsm-pro | Save/restore the session — **rewired to `context/progress-tracker.md`** |
| `review` | jsm-pro | 3-layer review (plan / system / production) — reads the code |
| `design-review` | frans (new) | Audits the UI against the [Web Interface Guidelines](https://github.com/vercel-labs/agent-skills/blob/main/skills/web-design-guidelines/SKILL.md) (a11y / interaction / UX) — fetches them fresh, reports `file:line` |
| `verify` | frans (new) | Runs the Next.js app and checks each `Done when` in the browser — gates Completed |
| `recover` | jsm-pro | Triage a failure: fix / hard reset / rethink |
| `imprint` | jsm-pro | Capture UI patterns into `context/ui-registry.md` |

**Deliberately left out:** `/architect` (replaced by `grill-with-docs`), `/to-prd` + `/to-issues` (replaced by `/to-plan` writing to `build-plan.md` — no issue tracker), mattpocock's own `/review` and `/handoff` (jsm `/review` and `/remember` cover them).

### What was adapted from the originals

- **Glossary moved into the folder.** Everything lives under `context/` — the glossary is `context/glossary.md`, not a separate root `CONTEXT.md`. (`domain-modeling`, `tdd`, `diagnosing-bugs`, `codebase-design`, `improve-codebase-architecture` updated.)
- **No issue tracker.** `/to-plan` writes each feature into `context/build-plan.md` (with a `Done when` checklist); `/implement`, `/review`, and `/verify` read it. No PRD, no issues.
- **One memory file.** `/remember` reads and writes `context/progress-tracker.md` instead of a separate `memory.md`, so the structured board and session memory never drift apart.
- **A runtime gate.** `/verify` runs the app and checks each `Done when` before a feature can move to Completed — `/review` alone only reads the code.

---

## Setup

### 1. Install the skills (once per machine)

Symlink each skill into your user-level Claude Code skills folder so they're available in every project:

```bash
for s in "C:/Users/PC/OneDrive/Documents/AI Skills/frans-skills/skills"/*/; do
  ln -s "$s" "$HOME/.claude/skills/$(basename "$s")"
done
```

(Or copy them, or symlink into a project's `.claude/skills/` instead for project-only use.)

**Also install the ui-ux-pro-max engine** — `/design-system` orchestrates it, so it must be present:

```bash
# via the Claude marketplace plugin, or the CLI:
npm install -g uipro-cli && uipro init --ai claude
```

See the [ui-ux-pro-max repo](https://github.com/nextlevelbuilder/ui-ux-pro-max-skill) for current install options. It needs **Python 3** for its search engine. Without it, `/design-system` will stop and tell you to install it rather than hand-rolling a design.

### 2. Bootstrap a project (once per project)

```bash
cp -r "C:/Users/PC/OneDrive/Documents/AI Skills/frans-skills/templates/context" your-project/context
```

Then merge `templates/CLAUDE.md` into your project's `CLAUDE.md` (create it if needed).

If the project has UI, make sure **shadcn** is initialized (`components.json` present) — `ui-rules.md` uses the [itshover](https://www.itshover.com/icons) animated-icon registry, installed per-icon via `npx shadcn@latest add …`. (itshover's license isn't stated on the site — confirm on its [GitHub repo](https://github.com/itshover/itshover) before shipping.)

### 3. Fill the static context files

Open each file in `your-project/context/` and replace the bracketed placeholders:
`project-overview.md`, `architecture.md`, `code-standards.md`, `library-docs.md`, `ui-tokens.md`, `ui-rules.md`, and the `build-plan.md` skeleton.

Leave `glossary.md`, `design-system.md`, `ui-registry.md`, and `progress-tracker.md` mostly empty — they grow as you build (via `/grill-with-docs`, `/design-system`, `/imprint`, and `/remember save`). `design-system.md` is the visual source of truth and `ui-tokens.md` / `ui-rules.md` are its enforced subset — `/design-system` keeps all three in sync, so you can leave the UI files for it to fill rather than hand-writing them up front.

### 4. Work the loop

Open with `/remember restore`, follow the loop in [WORKFLOW.md](./WORKFLOW.md), close with `/remember save`.

---

## Adding a feature later

Steps 1–3 are one-time setup. To add a feature to an existing project you skip them and run the same loop, starting at `/remember restore` → `/grill-with-docs`. Two practical notes:

- **Capture the idea so it doesn't get lost.** If an idea hits mid-build, drop a one-liner in the **Parking Lot** section of `progress-tracker.md` now, and grill + `/to-plan` it properly later. The idea is parked, not forgotten.
- **If the feature changes a locked decision**, grilling will surface it and `/grill-with-docs` will offer a new ADR — that's the system working, not a detour.

Only a genuinely **new project** needs the full setup (steps 1–3) again.

---

## Credit

Built on the work of [Matt Pocock](https://github.com/mattpocock/skills) and [JavaScript Mastery](https://github.com/frans-agst). Adapted into a single context-driven loop.
