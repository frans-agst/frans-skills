<!-- Build plan: features broken into phases. Each feature carries its own "Done when" acceptance criteria. -->
<!-- This is the plan (the denominator). Live status lives in progress-tracker.md (the numerator). -->
<!-- /to-plan appends feature entries here after a /grill-with-docs session. -->

# Build Plan

## Core Principle

[Describe your build philosophy here. e.g. Full page UI built with mock data first — verified visually before any logic is written. Then functionality is built and wired step by step. Every feature must be visible and testable before moving to the next. No invisible backend phases.]

---

## Phase Review — manual checklist (human)

<!-- Run through this yourself at the end of each phase before starting the next. It's a manual
     review — nothing in the workflow blocks on it, and the agent doesn't check these; you do. -->

At the end of a phase, check:

- [ ] Each feature's **Done when** still holds when you actually use it
- [ ] The features work together, not just each in isolation
- [ ] It delivers what this phase was meant to (the phase intent)
- [ ] Nothing in an earlier phase regressed
- [ ] It feels right — you'd ship this
- [ ] `progress-tracker.md` is current (Completed / Decisions / Known Issues)
- [ ] No Critical/Important Known Issue is being carried into the next phase unowned
- [ ] The next phase is still the right next move

---

## Phase 1 — [Name]

### 01 [Feature Name]

[One sentence describing what this feature builds.]

**UI:**

- [UI element or screen to build]
- [UI element or screen to build]

**Logic:**

- [Backend or data logic to implement]
- [Backend or data logic to implement]

**Done when:**

- [ ] [Observable, testable condition — what a user can do, or what is true, when this is complete]
- [ ] [Another criterion]

---

### 02 [Feature Name]

[One sentence describing what this feature builds.]

**UI:**

- [UI element or screen to build]

**Logic:**

- [Backend or data logic to implement]
- [Backend or data logic to implement]

**Done when:**

- [ ] [Observable, testable condition]
- [ ] [Another criterion]

---

## Phase 2 — [Name]

### 03 [Feature Name] — Full UI

[One sentence. Note if this is a UI-only step with mock data.]

**UI:**

- [UI element]
- [UI element]

**Done when:**

- [ ] [Observable, testable condition]

---

### 04 [Feature Name] — Logic

[One sentence. Note if this wires an existing UI to real data.]

**Logic:**

- [Logic step]
- [Logic step]

**Done when:**

- [ ] [Observable, testable condition]

---

## Phase 3 — [Name]

### 05 [Feature Name]

**UI:**

- [UI element]

**Logic:**

- [Logic step]

**Done when:**

- [ ] [Observable, testable condition]

---

## Phase 4 — [Name]

### 06 [Feature Name]

**UI:**

- [UI element]

**Logic:**

- [Logic step]

**Done when:**

- [ ] [Observable, testable condition]

---

## Feature Count

| Phase     | Name   | Features |
| --------- | ------ | -------- |
| 1         | [Name] | [n]      |
| 2         | [Name] | [n]      |
| 3         | [Name] | [n]      |
| 4         | [Name] | [n]      |
| **Total** |        | **[n]**  |

<!--
Notes
- "Done when" is this workflow's acceptance criteria — the part of a PRD worth keeping. /implement builds to it; /review checks against it.
- Keep criteria observable (what's true / what a user can do), not implementation steps.
- "Out of scope" for a feature is optional — add it as a bullet under the feature only when a boundary is worth pinning down.
-->
