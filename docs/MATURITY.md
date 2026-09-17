# Maturity ladder

<!--
GUIDANCE — MARKERS
  {{FILL: ...}} is a slot you must replace with real content.
  Every HTML comment block in this file starts with the word GUIDANCE and is
  an authoring instruction. Delete all of them once the file is filled.

GUIDANCE — WHAT THIS FILE OWNS
  How much an agent is allowed to do without a human in the loop, today and
  later. It holds the rung definitions (L0 through L3), the rule by which a
  project moves up a rung, the rule by which it falls back down, and which
  mechanical checks from `docs/capabilities/` gate each rung.

GUIDANCE — WHAT IT MUST NOT ABSORB
  How any individual check works or what it enforces — that is the owning
  spec card in `docs/capabilities/`. This file only names cards and states
  what they must subsume before a gate can retire.

GUIDANCE — ANTI-PATTERN THIS FILE EXISTS TO PREVENT
  Autonomy by drift: nobody decides an agent may now merge unreviewed work,
  it just gradually stops being reviewed, and the first serious failure has
  no one to attribute it to. Naming the current rung makes the level of
  trust a written, revisable claim. The mirror failure is autonomy by
  optimism — granting a rung because the agent has been doing well rather
  than because a check now catches the class of mistake the human gate was
  catching.

GUIDANCE — THE CORE RULE
  Trust is transferred to machinery, never to reputation. A human gate
  retires only when a mechanical check subsumes it and that check has been
  green for an agreed number of cycles. Write the number down; "for a while"
  is not a promotion criterion.

GUIDANCE — RIGHT-SIZING
  Every project starts at L0 and most stay below L2 for a long time. Keep
  the higher rungs written anyway: they are what make the current rung a
  deliberate position rather than the only thing anyone imagined.
-->

## Current rung

{{FILL: the rung this project is on today, and the single thing that would
have to become mechanical for it to move up.}}

## Rungs

<!--
GUIDANCE
  Four rungs, from fully human-gated to bounded autonomy. For each, state
  what the agent may do unsupervised, what still requires a human, and what
  evidence the agent must produce. Definitions must be concrete enough that
  two people reading a given change agree on which rung it was performed
  under.
-->

### L0 — {{FILL: rung name}}

{{FILL: what the agent may do unsupervised, what a human must approve, and
what evidence every change carries.}}

### L1 — {{FILL: rung name}}

{{FILL: same three, for this rung.}}

### L2 — {{FILL: rung name}}

{{FILL: same three, for this rung.}}

### L3 — {{FILL: rung name}}

{{FILL: same three, for this rung, plus the boundary of the classes of work
autonomy applies to.}}

## Promotion rule

{{FILL: the exact condition under which this project moves up a rung,
including the number of green cycles required and who confirms it.}}

## Demotion rule

<!--
GUIDANCE
  What sends the project back down: an escaped failure of a class a gating
  check was supposed to catch, a check disabled or made advisory, or a
  gating card falling out of `enforced`. Demotion has to be as mechanical as
  promotion, or the ladder only ever points one way.
-->

{{FILL: what triggers a fall-back, and what has to be true before the rung
can be re-earned.}}

## Gating capabilities

<!--
GUIDANCE
  One row per gate. The card must exist in `docs/capabilities/` and its
  status there must be `enforced` before the rung it gates can be claimed.
  Name only cards that exist; a row pointing at an imagined check is how a
  rung gets claimed on paper.
-->

| Rung | Gating card | What it must subsume |
| --- | --- | --- |
| {{FILL: L1}} | {{FILL: card name}} | {{FILL: the human gate it replaces}} |
| {{FILL: L2}} | {{FILL: card name}} | {{FILL: the human gate it replaces}} |
