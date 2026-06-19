---
name: design-review
description: "Audit a UI feature against the Web Interface Guidelines (accessibility, interaction, and UX best practices). Fetches the current guidelines fresh and reports file:line findings. Runs after /review (static) and before /verify (runtime); a failing audit blocks the move to Completed."
---

# Design Review

`/review` checks the feature against *your* plan and *your* design system. `/design-review` checks it
against the **Web Interface Guidelines** — the cross-industry baseline for accessibility, keyboard
and focus behavior, semantics, and interaction quality that no project's own context files fully
capture. It's the frontend half of "is it correct?" that lives between the static read (`/review`)
and the runtime check (`/verify`).

It's a thin wrapper around **vercel's web-design-guidelines**: it always fetches the *current*
guidelines rather than hardcoding a stale copy, then audits the feature's UI files against them.

Run it after `/review`, before `/verify`:

```
/design-review
```

To target specific files:

```
/design-review [file-or-glob]
```

---

## Step 1 — Fetch the guidelines fresh

WebFetch the current Web Interface Guidelines (the raw `command.md` in the
`vercel-labs/web-interface-guidelines` repo). Use what it returns — its rules **and** its output
format — as the source of truth for this run. Don't audit from memory; the guidelines change.

If the fetch fails (no connectivity), say so and stop — an audit against a half-remembered ruleset is
worse than no audit. Offer to retry or to skip to `/verify` with a note that the guideline audit was
not run.

## Step 2 — Find the feature's UI files

Read the current feature's `UI:` breakdown in `context/build-plan.md` to know which screens and
components are in scope. Then read the UI files that implement them — prefer files created or modified
while building this feature; fall back to the components directory. If it's unclear which files,
ask the developer rather than auditing the whole codebase.

## Step 3 — Audit against the fetched rules

Apply every rule the fetched guidelines define. Pay particular attention to the things a code-only
review and a token check both miss:

- **Keyboard & focus** — focus order, visible focus rings, focus traps, escape behavior
- **Semantics & a11y** — landmarks, labels, alt text, roles, contrast, reduced-motion
- **Interaction** — hit targets, disabled vs. busy states, optimistic vs. confirmed feedback
- **States** — empty / loading / error / success actually present (cross-check the `Done when` list)
- **Forms** — labels tied to inputs, error messaging, autofill, input types

Report findings in the **terse `file:line`** format the guidelines specify — one line per finding,
no prose padding.

## Step 4 — Report and route

```
## Design Review — [Feature Name]

[file:line] — [rule] — [what's wrong]
[file:line] — [rule] — [what's wrong]

Critical: [n]   Important: [n]   Minor: [n]
Verdict: [Clean — proceed to /verify] / [N issues — see Known Issues]
```

- This skill **reports; it does not fix** — same contract as `/review`. The developer decides.
- Record **Critical** and **Important** findings under **Known Issues** in
  `context/progress-tracker.md` so they aren't lost.
- A feature with unresolved Critical findings does **not** move to Completed — fix (hand to
  `/diagnosing-bugs` if it's a real defect) and re-run, or have the developer explicitly accept a
  finding as out of scope.

---

## The standard

A feature can match your plan and your tokens perfectly and still be unusable with a keyboard or
invisible to a screen reader. Your context files can't encode the whole accessibility baseline — so
this step borrows one that's maintained, and checks against it every time.
