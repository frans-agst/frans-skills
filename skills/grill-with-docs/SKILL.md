---
name: grill-with-docs
description: A relentless three-lens interview (PM, senior dev, UI/UX) that sharpens a plan or design until it's non-generic, and creates docs (ADR's and glossary) as we go.
disable-model-invocation: true
---

Run a `/grilling` session, using the `/domain-modeling` skill.

`/grilling` drives the interview — three rounds (Product → Engineering → Experience), one question at
a time, hunting down generic defaults and gated on a readiness checklist. `/domain-modeling` runs
alongside it: as each answer lands, capture resolved terms into `context/glossary.md` and offer an ADR
whenever a decision is hard to reverse, surprising, and the result of a real trade-off. The grilling
produces the sharpness; domain-modeling makes it durable.
