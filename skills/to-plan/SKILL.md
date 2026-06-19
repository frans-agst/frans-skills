---
name: to-plan
description: "Turn the current grilling conversation into a feature entry in context/build-plan.md. No interview — just synthesis."
disable-model-invocation: true
---

Take the current conversation (usually a just-finished `/grill-with-docs` session) and the codebase understanding, and write a **feature entry** into `context/build-plan.md`. Do NOT interview the user again — synthesize what you already know. This is the no-issue-tracker analog of a PRD: the feature's "what to build" lives in the build plan, while the "why" and the decisions already live in `context/glossary.md` and `docs/adr/`.

## Process

1. **Read the existing plan.** Open `context/build-plan.md` to see the phases, the feature numbering, and the "Core Principle". Use the project's glossary vocabulary (`context/glossary.md`) throughout, and respect any ADRs in the area you're touching.

2. **Place the feature.** Decide which phase it belongs to and what its number is. If it opens a new area of work, propose a new phase. Confirm placement with the user before writing — one question: _"This looks like Phase N, feature NN — or should it start a new phase?"_

3. **Write the entry** using the build-plan feature format. Keep it concise — this is a plan, not a spec. Don't include file paths or code snippets (they go stale); the grilling conversation and ADRs hold the detail.

```
### NN [Feature Name]

[One sentence describing what this feature builds.]

**UI:**

- [UI element or screen]

**Logic:**

- [Backend or data logic]

**Done when:**

- [ ] [Observable, testable condition — what a user can do, or what is true, when complete]
- [ ] [Another criterion]
```

   - **Done when** is the most important part — it's the acceptance criteria `/implement` builds to and `/review` checks against. Make each criterion observable, not an implementation step.
   - **For UI features, the `Done when` list must enumerate the things that otherwise get skipped:** the empty / loading / error / success states, the responsive breakpoints actually supported, keyboard + a11y (focus order, labels, contrast), and any icons. `/design-review` and `/verify` check these — so name them here or they won't be gated.
   - Add an **Out of scope** bullet under the feature only if a boundary is worth pinning down.

4. **Update the feature count** table at the bottom of `context/build-plan.md`.

5. **Mark it Up Next.** Add the feature to the **Up Next** section of `context/progress-tracker.md` so the next session picks it up. **If the feature has a `UI:` section, tag it _needs-design_** — it goes through `/design-system` (which generates the design before any UI is built) before `/implement`. Pure-logic features skip straight to `/implement`. Either way, do not start building — that's `/implement`.

## Boundaries

- One feature (or one tight cluster) per run. If the grilling covered several independent features, write them as separate entries and tell the user how you split them.
- Do not modify other features' entries. Append; don't rewrite the plan.
- If there was no grilling and the conversation is thin, say so and suggest `/grill-with-docs` first rather than inventing requirements.
