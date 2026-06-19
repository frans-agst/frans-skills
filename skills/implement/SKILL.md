---
name: implement
description: "Implement the current feature from the build plan."
disable-model-invocation: true
---

Implement the feature the user is pointing at. The source of truth is the current feature in `context/build-plan.md` plus whatever was agreed in the `/grill-with-docs` session that preceded this — not a PRD or issue tracker.

Before starting:

- Read `context/build-plan.md` to confirm exactly which feature (and its UI / Logic / done-criteria) you are building.
- Read `context/glossary.md` for naming, `context/architecture.md` and `context/code-standards.md` for the rules you must follow, and check `docs/adr/` for decisions in the area you're touching.
- Mark the feature as **In Progress** in `context/progress-tracker.md`.

While building:

- Use /tdd where possible, at the seams agreed during grilling.
- After each UI component, run /imprint so it lands in `context/ui-registry.md`.
- Run typechecking regularly, single test files regularly, and the full test suite once at the end.

Once done:

- Use /review to review the work (static — reads the code).
- Use /verify to run the app and confirm each **Done when** criterion actually holds. Only move the feature to Completed if /verify passes; if it fails, leave it In Progress and hand to /recover.
- Update `context/progress-tracker.md`: move the verified feature to **Completed**, log any decisions under **Decisions Made**, and record anything unfinished or broken under **Known Issues**.
- Commit your work to the current branch.
