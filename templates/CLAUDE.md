<!-- Drop this block into your project's CLAUDE.md (or AGENTS.md / .cursorrules). -->
<!-- It tells the agent to load context every session and keep the board alive. -->

# Working agreement

## Context

- **Orientation comes from `/remember restore`** — it loads `context/progress-tracker.md`, the active phase of `context/build-plan.md`, and `context/glossary.md`. Read the rest of `context/` and specific ADRs **on demand**, when a task reaches that area: `context/library-docs.md` before using a library, `architecture.md` / `code-standards.md` before writing code there, the UI files before UI work, a specific `docs/adr/` file before touching the decision it records. Don't preload the whole folder — it's large and most of it won't be relevant to a given task.
- `context/glossary.md` is the project's language — use those terms exactly when naming things.
- `context/architecture.md`, `context/code-standards.md`, `context/design-system.md`, `context/ui-rules.md`, and `context/ui-tokens.md` are rules, not suggestions. Don't violate them; if one is wrong, say so before working around it.
- `context/design-system.md` is the visual source of truth; `context/ui-rules.md` and `context/ui-tokens.md` are its enforced subset. Don't invent UI look-and-feel — it comes from `/design-system`.

## Session ritual (non-negotiable)

- **Start every session** by running `/remember restore` — it loads the orientation set (tracker + active plan phase + glossary), indexes the rest of `context/` for on-demand reading, then confirms where we are before doing anything.
- **End every session** by updating `context/progress-tracker.md` and running `/remember save`.

## How we work

- Before building anything non-trivial: `/grill-with-docs` to align, then `/to-plan` to write the feature into `context/build-plan.md`.
- For UI features: `/design-system` before building — it generates the design (via ui-ux-pro-max) into `context/design-system.md` and syncs `ui-tokens.md` / `ui-rules.md`. Don't build UI without a design pass.
- Build with `/implement` (uses `/tdd` and `/codebase-design` at agreed seams).
- After each UI component: `/imprint` (keeps `context/ui-registry.md` current).
- After each feature: `/review` (reads the code), then `/design-review` (audits the UI against the Web Interface Guidelines), then `/verify` (runs the app and checks each `Done when`). A feature only moves to Completed after `/verify` passes.
- When something breaks: `/recover` first (it triages), then `/diagnosing-bugs` if it's a real bug.

## The two files that track progress

- `context/build-plan.md` — the plan (phases → features with done-criteria). The denominator.
- `context/progress-tracker.md` — live status (Completed / In Progress / Up Next / Blocked / Known Issues / Decisions / Session Notes). The numerator. **Update it every session.**
