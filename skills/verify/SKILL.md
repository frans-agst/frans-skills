---
name: verify
description: Run the app and confirm a feature actually works against its "Done when" criteria. Use after /review, when the user asks to verify a feature, confirm a fix works, or check that something works in the browser before marking it done.
---

# Verify

`/review` reads the code. `/verify` runs the app. A feature isn't done because it compiles and reads correctly — it's done when its **`Done when`** criteria actually hold in the running app. This step is the runtime half of "is it correct?", and it gates the move to **Completed** in `context/progress-tracker.md`.

This workflow assumes a **Next.js** app. Adjust the run command if the project differs.

## Step 1 — Establish what "working" means

Read the feature's **`Done when`** criteria from `context/build-plan.md`. Those are your pass/fail checklist — verify against them, not against a vague sense that it "looks fine". If a criterion is too vague to check at runtime, say so and propose a sharper one.

Also note the analytics events or key flows the feature touches (from `context/project-overview.md` / `code-standards.md`) if they're part of the criteria.

## Step 2 — Run the app

Detect the package manager from the lockfile (`pnpm-lock.yaml` → pnpm, `yarn.lock` → yarn, `bun.lockb` → bun, else npm) and start the dev server in the background:

```
<pm> run dev        # e.g. npm run dev — Next.js defaults to http://localhost:3000
```

Wait until it's serving (poll the URL). Keep the **server log** visible — Next.js surfaces RSC errors, server-action failures, and build errors there, not in the browser.

## Step 3 — Exercise the feature

Drive the **real path a user takes**, in this order of preference:

1. **Headless browser** (Playwright/Puppeteer) if available — navigate the route, perform the interactions the criteria describe, and assert on the DOM. This is the tightest signal for UI features.
2. **HTTP** — for API routes / server actions / data flows, hit the endpoint with a realistic request and check the response.
3. **Human-in-the-loop** — only when something genuinely needs a person (OAuth redirect, email, payment). Give the user exact steps and ask what they observed. Don't guess.

Use the project's seams; don't build elaborate harnesses for a simple check.

## Step 4 — Watch for runtime failures

While exercising the feature, actively check:

- **Browser console** — errors and warnings, including React **hydration mismatches** (a common Next.js footgun) and failed requests in the network tab.
- **Server log** — unhandled errors, server-action/RSC failures, 500s.
- **Empty / loading / error states** — trigger them, don't just assume they render.
- **The unhappy path** — bad input, missing data, unauthorized access.

### Frontend pass (UI features)

`/design-review` already audited the feature against the Web Interface Guidelines statically — here you confirm it **holds when the app is running**:

- **Responsive** — resize through the breakpoints the `Done when` list names; check nothing overflows, collapses, or overlaps.
- **Keyboard** — tab through the feature: focus order is sane, focus is visible, nothing is a trap, Escape works where expected.
- **Screenshots** — capture the key screens/states as a baseline (attach or save them). A textual class check (`/imprint`) can't catch visual drift; a screenshot can.

## Step 5 — Report a per-criterion verdict

```
## Verify — [Feature Name]

| Done when | Result |
| --------- | ------ |
| [criterion 1] | ✅ Pass / ❌ Fail — [what you observed] |
| [criterion 2] | ✅ Pass / ❌ Fail — [what you observed] |

**Console/server:** [clean, or list errors + where]
**States checked:** [empty / loading / error / unhappy path]

**Verdict:** [All criteria pass — ready to mark Completed] / [N criteria failing — see below]
```

## Step 6 — Close out

- **Stop the dev server** you started.
- **All pass:** tell the user the feature meets its criteria. The move to **Completed** in `context/progress-tracker.md` can now happen (that's `/implement`'s job, or do it here if asked).
- **Any fail:** do **not** mark it Completed. Leave it **In Progress**, record the failing criterion under **Known Issues**, and hand to `/recover` (which triages) or `/diagnosing-bugs` (for a confirmed bug). A failed verify is the system working — better here than in production.

## The standard

Working in your head is not working in the browser. Verify against the criteria, with the app actually running, before anything is called done.
