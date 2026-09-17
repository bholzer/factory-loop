# Capabilities

<!--
GUIDANCE — MARKERS
  {{FILL: ...}} is a slot you must replace with real content.
  Every HTML comment block in this file starts with the word GUIDANCE and is
  an authoring instruction. Delete all of them once the file is filled.

GUIDANCE — WHAT THIS FILE OWNS
  The register of spec cards in this directory and, for each, its status and
  where it is enforced. Status lives here and nowhere else: the cards
  themselves are specifications and say nothing about their own state.
  `CARD_FORMAT.md` in this directory defines how a card is written and what
  a status transition requires.

GUIDANCE — WHAT IT MUST NOT ABSORB
  Any invariant's content — that is the card. Any autonomy rung a card gates
  — that is `docs/MATURITY.md`. This file is an index.

GUIDANCE — ANTI-PATTERN THIS FILE EXISTS TO PREVENT
  Status drift in both directions: a check quietly built but still listed as
  `specced`, so nobody runs it, and a check listed as `enforced` after being
  disabled or made advisory, so a maturity rung rests on nothing. One table,
  one owner, changed in the same commit as the mechanism.
-->

## Register

<!--
GUIDANCE
  One row per card file in this directory. Statuses are `specced`, `built`,
  or `enforced`; `built` requires the demonstrated failing case that
  `CARD_FORMAT.md` describes. "Enforced at" names the concrete place the
  check runs — a command, a hook, a CI job — and reads `—` while the status
  is `specced`.
-->

| Card | Status | Enforced at |
| --- | --- | --- |
| {{FILL: card-name}} | {{FILL: specced}} | {{FILL: — }} |
| {{FILL: card-name}} | {{FILL: specced}} | {{FILL: — }} |
