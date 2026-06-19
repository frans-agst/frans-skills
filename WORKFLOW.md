# The Workflow

A context-driven engineering loop that combines three sources:

- **mattpocock/skills** — depth: relentless alignment, domain glossary + ADRs, TDD with deep modules, rigorous bug diagnosis.
- **jsm-pro-skills** — continuity & discipline: cross-session memory, structured review, failure triage, UI consistency.
- **context-driven-dev** — the substrate: a `context/` folder the agent reads every session, with the two files that make progress legible.

It deliberately **does not use an issue tracker.** The plan and the status live in two files instead.

> **Stack:** the UI context templates and the `/verify` run command assume **Next.js + Tailwind v4 + shadcn** (shadcn for the itshover icon registry). `/design-system` also needs the **ui-ux-pro-max** engine installed (see README). Everything else is stack-agnostic.

---

## The two files that answer "where are we?"

| File | Role | Update when |
|---|---|---|
| `context/build-plan.md` | The **plan** — phases → features, each with a UI / Logic breakdown and a **Done when** checklist. The *denominator* ("12 features across 4 phases"). | Scope changes |
| `context/progress-tracker.md` | The **status** — Completed / In Progress / Up Next / Parking Lot / Blocked / Known Issues / Decisions / Session Notes. The *numerator* ("5 done, on #6"). | **Every session** |

Together they tell you "5 of 12 done, working on auth" at a glance. That was the whole problem to solve.

---

## The session ritual (non-negotiable)

> **Open** every session with `/remember restore`.
> **Close** every session by updating `context/progress-tracker.md` + `/remember save`.

Every time. A board updated *sometimes* is a board you can't trust.

---

## The per-feature loop

```
  /remember restore                         ← start: reload tracker + context, confirm where we are
        │
        ▼
  /grill-with-docs                          ← align before building; writes context/glossary.md + docs/adr/
        │
        ▼
  /to-plan                                  ← synthesize the grilling into a context/build-plan.md
        │                                      feature entry (UI / Logic / Done when) + marks it "Up Next"
        ▼
  /design-system   (UI features only)        ← generate the design BEFORE building: runs ui-ux-pro-max,
        │                                      writes context/design-system.md, syncs ui-tokens/ui-rules
        ▼
  /implement   (uses /tdd, /codebase-design) ← build from the build-plan entry, at agreed seams
        │  └─ /imprint after each UI component → context/ui-registry.md
        ▼
  /review                                   ← 3-layer check (plan / system / production), reports only — reads the code
        │
        ▼
  /design-review   (UI features only)        ← audit the UI against the Web Interface Guidelines
        │                                      (a11y / interaction / UX); fetches them fresh, reports file:line
        ▼
  /verify                                   ← run the app; confirm each "Done when" holds in the browser
        │                                      gates the move to Completed (fail → /recover or /diagnosing-bugs)
        ▼
  update context/progress-tracker.md        ← feature → Completed, log decisions, note issues
        │
        ▼
  /remember save                            ← end: writes tracker + next action
```

**Why `/grill-with-docs` instead of `/architect`:** both align before building, but grilling is sharper *and* leaves durable artifacts — `context/glossary.md` and ADRs — instead of just chat output. Grilling produces no plan, which is why `/to-plan` writes the build-plan entry right after.

**Why `/design-system` before `/implement`:** the UI context files (`ui-rules.md`, `ui-tokens.md`) are written as if a design was *already delivered* — but nothing in the loop delivered it. `/design-system` is that step: it runs the **ui-ux-pro-max** engine to decide the look (pattern, palette, typography, effects) once, up front, and captures it into `context/design-system.md`. Without it, the design gets decided implicitly one component at a time — the exact drift `/imprint` exists to clean up after. Decide it once, then build to it. (`/design-review` is the matching gate on the way out: it audits the built UI against the Web Interface Guidelines, the accessibility/UX baseline your own context files don't fully capture.)

---

## Adding a feature to an existing project

Same loop — you just skip setup (the `context/` folder already exists) and start at `/remember restore` → `/grill-with-docs`. Two notes:

- **Park raw ideas.** An idea that hits mid-build goes in the **Parking Lot** of `progress-tracker.md`, to be grilled and `/to-plan`-ed later — not built on the spot.
- **Grilling guards your decisions.** Because the glossary and ADRs already exist, `/grill-with-docs` cross-references them and pushes back where the new idea conflicts — and offers a new ADR if it changes a locked decision.

---

## When something breaks

```
/recover   → triages the failure mode:
   ├─ Targeted fix  → hand to /diagnosing-bugs (build a tight, red-capable feedback
   │                   loop FIRST, then hypothesise → fix → regression-test)
   ├─ Hard reset    → /remember save the salvage note → new session → /remember restore
   └─ Rethink       → back to /grill-with-docs to re-align → update ADRs
```

`/recover` decides *which* kind of failure you have; `/diagnosing-bugs` is the deep loop for the "targeted fix" branch.

---

## Maintenance (every few days)

- `/improve-codebase-architecture` — find deepening opportunities before the codebase turns into a ball of mud.
- `/imprint audit` — scan for UI drift and re-baseline `context/ui-registry.md`.

---

## Skill cheat-sheet

| When | Skill | Touches |
|---|---|---|
| Start of session | `/remember restore` | reads `context/` + `progress-tracker.md` |
| Before building anything | `/grill-with-docs` | writes `context/glossary.md`, `docs/adr/` |
| Turn the grilling into a plan entry | `/to-plan` | `context/build-plan.md` + `progress-tracker.md` |
| Design a UI feature (before building) | `/design-system` | `context/design-system.md`, syncs `ui-tokens.md` / `ui-rules.md` |
| Build | `/implement` (+ `/tdd`, `/codebase-design`) | `src/` |
| After a UI component | `/imprint` | `context/ui-registry.md` |
| After a feature (static) | `/review` | reports against `context/` + plan |
| After a feature (UI guidelines) | `/design-review` | audits UI vs. Web Interface Guidelines |
| After a feature (runtime) | `/verify` | runs the app, checks each `Done when` |
| Something broke | `/recover` → `/diagnosing-bugs` | — |
| End of session | update tracker + `/remember save` | `context/progress-tracker.md` |
| Every few days | `/improve-codebase-architecture`, `/imprint audit` | `src/`, `ui-registry.md` |

---

## File map of a project using this workflow

```
your-project/
├── CLAUDE.md                  # the ritual block (templates/CLAUDE.md)
├── context/                   # copied from templates/context/, filled in before building
│   ├── project-overview.md    # } fill once
│   ├── architecture.md        # } before you
│   ├── code-standards.md      # } start building
│   ├── library-docs.md        # }
│   ├── build-plan.md          # the plan
│   ├── glossary.md            # grill-with-docs writes this
│   ├── design-system.md       # /design-system writes this — the visual source of truth
│   ├── ui-tokens.md           # } enforced subset of design-system.md
│   ├── ui-rules.md            # }
│   ├── ui-registry.md         # /imprint keeps this alive
│   └── progress-tracker.md    # ★ the heartbeat — read + written every session
├── docs/adr/                  # ADRs (decisions too big for the tracker)
└── src/...
```

See [README.md](./README.md) for setup.
