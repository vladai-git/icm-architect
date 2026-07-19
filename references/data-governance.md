# Data governance — an optional layer

**Apply this only when your workspace holds shared, regulated, or sensitive data** — a team brain read by many people, client or personal records, anything with a compliance obligation. A private notes vault or a solo pipeline does not need it; leaving it out costs nothing. This layer formalizes three things ICM already reaches for informally, drawn from DAMA-DMBOK practice, without adding a required step to any form.

## 1. Authoritative source — "one home per fact," with teeth

Invariant 8 already says *one home per fact, a link beats a copy*. Governance makes that home **declared**, not assumed: for any fact or term that appears in more than one place, one file is the source of truth and the rest link to it.

- In a **knowledge bundle** or **context map**, add `source_of_truth: true` (or a `source:` pointer) to frontmatter so a reader — human or agent — can tell the master from a mirror.
- Keep a small `_glossary/` when terms drift across folders: one entry per term, its definition, and its authoritative home. A term defined twice, differently, is the bug this catches.
- The dedup check in the walk test then has an answer to *"which of these two is the real one?"* — the one marked authoritative.

## 2. Sensitivity classification — before you point one agent at everything

ICM's defining move is *one agent walks the whole workspace*. That makes **what the agent can read** a first-class concern. The knowledge-bundle form already has `access_tier` gating "what may leave the machine"; this generalizes it to any form.

- Give sensitive nodes/folders a tier in frontmatter (e.g. `sensitivity: public | internal | restricted`).
- State the rule once in the entry file or `_meta/`: *what an agent may load, quote, or send outward at each tier.* Patterns abstracted from restricted sources may travel; raw restricted content may not.
- Before connecting an agent to the workspace, confirm nothing above your chosen tier sits where an untiered walk would slurp it. This is a **walk-test add-on**: *"walking cold, could I read or exfiltrate something above its tier?"* If yes, the structure — not the prompt — is wrong.

## 3. Provenance on generated outputs

When a factory run emits a file, record where it came from. A generated artifact is only trustworthy if you can trace it back to its inputs.

- Add `source:` / `generated_from:` frontmatter (input paths or a run id) to factory outputs.
- This turns invariant 9 (*the filesystem is the state machine*) into an auditable one: status is not only "what exists" but "what produced it."

## What this is not

- **Not mandatory.** No form requires it; it is a layer you opt into when data sensitivity earns it.
- **Not heavyweight.** Three frontmatter conventions and one glossary folder, not a compliance program. If a convention isn't queried or enforced, drop it (same guardrail as any ICM frontmatter field).
- **Not a substitute for the reference-integrity gate** (see [reference-integrity.md](reference-integrity.md)) — that protects *moves*; this protects *meaning, access, and origin*.
