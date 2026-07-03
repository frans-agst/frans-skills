---
name: design-system
description: "Generate the visual design for a UI feature BEFORE building it — first INTERROGATES the design intent (feel, references, signature, anti-patterns) as a UI/UX expert until the brief is non-generic, then runs ui-ux-pro-max to produce a design system and captures it into context/. The design-creation half of the loop; runs between /to-plan and /implement for UI features."
disable-model-invocation: true
---

# Design System

`/to-plan` says *what* to build. This skill decides *how it should look* — before a line of UI is
written. It is the design-creation step the rest of the loop assumes already happened: `ui-rules.md`
and `ui-tokens.md` are written as if "a design was delivered." This is where that design gets
delivered.

It works in two moves: first it **interrogates the design intent** like a UI/UX expert who refuses to
ship a template — feel, references, signature, anti-patterns — until the brief is specific enough that
the output *can't* be generic. Then it runs the **ui-ux-pro-max** engine (the generator) on that brief
and captures the output into the `context/` substrate, so `/implement`, `/imprint`, and `/review` all
read the same files they already read.

> **Requires ui-ux-pro-max installed.** See README setup. If it isn't installed, stop and say so —
> don't invent a design system by hand; that's the whole reason this engine exists.

> **Stack:** assumes Next.js + Tailwind v4 + shadcn (for itshover icons). Adjust if the project differs.

Run this **for UI features only**, right after `/to-plan` marks the feature Up Next:

```
/design-system
```

To target a specific feature or page explicitly:

```
/design-system [feature name or page]
```

---

## Step 1 — Establish what's being designed, and what already exists

Read, in this order:

- The feature's **`UI:`** breakdown and **`Done when`** criteria in `context/build-plan.md` — that's
  the brief. Note every screen, state, and component it names.
- `context/glossary.md` — name things in the project's language.
- The **existing** design, so the new work *extends* it instead of contradicting it:
  - `context/design-system.md` — the MASTER (global pattern, palette, typography). If it already has
    content, you are adding a feature *under* an established system, not starting fresh.
  - `context/ui-tokens.md` and `context/ui-rules.md` — the enforced subset already in use.
  - `context/ui-registry.md` — components already built (match them).

If `context/design-system.md` is still the empty skeleton, this is the **first** design pass — the
engine establishes the global system. If it has content, this is a **per-feature** pass — generate
only what's new and reconcile it against the established system.

---

## Step 2 — Interrogate the design intent

Before the engine runs, grill the design. ui-ux-pro-max is only as good as the brief you hand it —
feed it *"a settings page"* and you get a settings page that looks like every settings page. The
whole point of this step is to arrive at a brief so specific the output **couldn't** be generic.

Run a **`/grilling` pass scoped to the Experience lens** (Round 3 of that skill), grounded in the
product answers from the `/grill-with-docs` session that produced this feature. One question at a
time, recommend an answer each time, and apply grilling's **kill-generic** doctrine without mercy — a
design brief is exactly where genericness hides. When I reach for *"clean and modern,"* push back:
that describes ten thousand dashboards, not this one.

Gate it on this design-readiness checklist — show it as boxes flip, and do **not** run the engine
until every box is checked (or I say **"ship it"**):

```
DESIGN READINESS — n/7
[ ] Emotional register — 2-3 adjectives for the feel, AND what it's NOT
[ ] Anti-references — what it must NOT resemble (name "generic shadcn dashboard" if that's the risk)
[ ] North-star references — 1-3 concrete products, and what *specifically* about each
[ ] Signature moment — the one screen/interaction that carries the personality
[ ] Density & rhythm — spacious vs dense, calm vs energetic, where the whitespace goes
[ ] Motion & voice — how it moves, and how the microcopy sounds
[ ] The distinctive commitment — one opinionated choice a template would never make
```

The answers to these seven **are** the brief for Step 3 — not a vague phrase, a paragraph with a
point of view. On a per-feature pass, this interrogation is lighter (the register and references are
already locked in `context/design-system.md`); focus it on the signature and the distinctive
commitment *for this feature* without contradicting the established system.

---

## Step 3 — Run ui-ux-pro-max

Feed the engine the **brief you built in Step 2** — the design-readiness answers — plus the project's
domain and stack. Use the install's normal invocation — for example the direct script:

```
python3 .claude/skills/ui-ux-pro-max/scripts/search.py "<feature UI description>" \
  --design-system --domain "<product domain>" --stack "nextjs-tailwind-shadcn" --persist
```

or the auto-activated skill / `/ui-ux-pro-max <request>` form, whichever the install exposes.

Pass the **Step 2 brief in full** — the feel, anti-references, north-stars, signature, density, motion,
and the distinctive commitment — not a vague phrase. The richer and more opinionated the request, the
better the reasoning, and the interrogation is what made it rich. The engine returns: a **pattern**, a
**style**, a **color palette**, a **typography pairing**, **key effects**, **anti-patterns to avoid**,
and a **pre-delivery checklist**. Reconcile its output against your Step 2 answers — if it hands back
something that contradicts the register or drifts toward an anti-reference, push it again rather than
accepting a generic result the interrogation was meant to prevent.

On the **first** pass, let it generate the global system. On **per-feature** passes, constrain it to
the established palette/typography in `context/design-system.md` and only let it add what's genuinely
new (a new component pattern, a new state) — surface any conflict with the locked system rather than
silently overriding it.

---

## Step 4 — Capture the output into context/

The engine's native persistence is a `design-system/MASTER.md` + `pages/*.md` folder. **This project
keeps one home: `context/`.** Redirect the output there.

1. **`context/design-system.md`** — the source of truth.
   - First pass: write the MASTER (pattern, palette, typography, effects, anti-patterns).
   - Every pass: append a **per-feature section** (`## [Feature] — design`) with what's specific to
     this feature, mirroring ui-ux-pro-max's MASTER+pages split inside one file. Append; don't
     overwrite the MASTER.

2. **`context/ui-tokens.md`** — sync the concrete values down. Turn the palette + typography into the
   `@theme` token block. These are what `/implement` actually applies.

3. **`context/ui-rules.md`** — sync the component-level rules (cards, buttons, badges, inputs,
   spacing, the anti-patterns as "Do Nots"). Keep the **Icons** section pointing at itshover.

The hierarchy: `design-system.md` is the *why and the whole*; `ui-tokens.md` / `ui-rules.md` are the
*enforced subset* the build reads. They must agree — if you change one, sync the others.

---

## Step 5 — Icons

This project uses **itshover** animated icons (a shadcn registry). When the design calls for icons,
record which ones in the per-feature section and confirm `context/ui-rules.md` documents the install
pattern (`npx shadcn@latest add https://itshover.com/r/<icon>.json`). Don't hand-roll SVG icons when
an itshover icon fits.

---

## Step 6 — Confirm and hand off

Report what was generated:

```
Design system → context/design-system.md

Pattern:     [pattern]
Palette:     [key colors]
Typography:  [font pairing]
Effects:     [key effects]
Signature:   [the one distinctive moment/interaction from Step 2]
Icons:       [itshover icons chosen, if any]
Synced:      context/ui-tokens.md, context/ui-rules.md

Anti-patterns to avoid: [the engine's flagged anti-patterns + the Step 2 anti-references]

Ready for /implement.
```

If the engine's output conflicts with a locked decision in `context/design-system.md` or an ADR, say
so — that's a decision for the developer (and possibly a new ADR via `/grill-with-docs`), not
something to paper over.

---

## The standard

The design exists before the code does — and it is *specific* before it exists. A feature built
without a design pass has its look decided implicitly, one component at a time, which is exactly the
drift `/imprint` exists to clean up after. But a design pass fed a lazy brief is barely better: it
produces a competent, forgettable interface that could belong to any product. The interrogation in
Step 2 is the difference. Decide the design once, here — and decide it with a point of view — then
build to it.
