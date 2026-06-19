---
name: remember
description: Save where the build stands at the end of a session, and restore full context at the start of the next one. The single living record is context/progress-tracker.md.
disable-model-invocation: true
---

AI has no memory between sessions. Every new session starts blank. This skill fixes that — and in this workflow there is **one** living record, not two: `context/progress-tracker.md`. `/remember save` updates it at the end of a session; `/remember restore` reads it (plus the rest of `context/`) at the start of the next. The tracker is the source of truth for "where are we"; this skill is the save/restore ceremony that keeps it honest.

## Security Boundary

This skill must never persist secrets. If any sensitive value appears in the conversation or context, do not copy it into `context/progress-tracker.md`.

Sensitive data includes (non-exhaustive):

- API keys, access tokens, refresh tokens, session tokens
- Passwords, passphrases, one-time codes, private keys, certificates
- Cookies, auth headers, connection strings, webhook secrets
- Any credential-like value or secret-looking string

If a detail is useful but sensitive, store a redacted placeholder instead (for example: `[REDACTED_API_KEY]`). If unsure whether something is sensitive, treat it as sensitive and omit or redact it.

## How to Invoke

**To save at end of session:** `/remember save`
**To restore at start of new session:** `/remember restore`

If the developer just runs `/remember` without specifying — ask them which one they need.

---

## Save Mode

When the developer runs `/remember save`, update `context/progress-tracker.md` to reflect exactly where the build now stands. You are not writing a transcript — you are bringing the board up to date so a fresh session (or a colleague who knows nothing about today) could continue without losing anything.

### What to update

Read the current `context/progress-tracker.md` first, then revise these sections in place:

- **Header** — update `Last updated`, `Current phase`, and `Overall status`.
- **Completed** — move anything finished this session here. Be precise: not "built the auth flow" but "created `app/(auth)/login/page.tsx`, `callback/page.tsx`, `middleware.ts`; Google + GitHub OAuth working end to end."
- **In Progress** — what is partially done right now, with enough detail to resume.
- **Up Next** — the very next thing to do, specific enough to start immediately.
- **Blocked** — anything waiting on a decision or dependency, with what's blocking it.
- **Parking Lot** — raw ideas that surfaced but aren't planned yet. Keep them; don't promote them to Up Next until they've been grilled and run through `/to-plan`.
- **Known Issues** — bugs or gaps discovered but not yet fixed (Issue / Severity / Status).
- **Decisions Made** — choices future work depends on. For a decision that is hard to reverse, surprising, and a real trade-off, write a one-line entry here and create a full ADR in `docs/adr/` — link the two. Don't duplicate the ADR's contents into the tracker.
- **Session Notes** — append a dated entry: what was done, what broke, what to watch for.

### What not to capture

- Implementation details visible in the code
- Anything already in `context/glossary.md`, `context/architecture.md`, or an ADR — link, don't copy
- The blow-by-blow of *how* something was built — only what was built and what was decided
- Any secret or credential-like value

### Safety check before writing

Before writing, run a final pass to ensure no sensitive value is present. Remove or redact anything sensitive.

### Confirm before overwriting

`context/progress-tracker.md` accumulates across the whole build — never blow it away. Update sections in place: move items between Completed / In Progress / Up Next, and **append** to Decisions Made and Session Notes rather than replacing them. Show the developer a short summary of what you changed, then write.

After writing, confirm:

```
Progress tracker updated.
- Moved to Completed: [items]
- Now In Progress: [items]
- Next session starts with: [item]

Next session: run /remember restore to pick up from here.
```

---

## Restore Mode

When the developer runs `/remember restore` at the start of a new session:

### Step 1 — Read the board

Read `context/progress-tracker.md`. If it does not exist, tell the developer:

```
No context/progress-tracker.md found.

Either this is the first session, or context/ hasn't been set up yet.
Copy the context template in, then run /remember save at the end of a session.
```

### Step 2 — Read the rest of the context

Then read the rest of `context/` and the project glossary so you have the full picture:

- `context/glossary.md` — the project's language
- `context/project-overview.md`, `context/architecture.md`, `context/build-plan.md`, `context/code-standards.md`, `context/library-docs.md`
- `context/ui-tokens.md`, `context/ui-rules.md`, `context/ui-registry.md`
- `docs/adr/` — decisions already locked
- The agent instruction file if present (`CLAUDE.md`, `AGENTS.md`, `.cursorrules`, etc.)

Do not scan beyond these. Never surface raw secrets; summarise any in redacted form only.

### Step 3 — Confirm what was restored

Do not start building. Summarise so the developer can verify you understood correctly:

```
Restored. Here is where we are:

**Current phase:** [phase] — [overall status]
**Last session:** [what was completed]
**In progress:** [what's partial]
**Decisions locked:** [key decisions / ADRs]
**Next up:** [what to start with]

Is this correct? Say yes to continue, or correct anything before we proceed.
```

Only after the developer confirms does the session continue.

### If the tracker is incomplete or unclear

```
I found context/progress-tracker.md but some context seems missing — [what is unclear].

Continue with what we have, or fill the gaps first?
```

Do not guess. Surface the gap and let the developer decide.

---

## The Rule

Every session ends with `/remember save`. Every session starts with `/remember restore`.

That is the whole system. A board updated sometimes is a board you cannot trust.
