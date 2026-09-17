# 0002 — The plan convention is the ExecPlan document adopted verbatim, plus local House Rules

## Decision

`plans/PLANS.md` carries the external ExecPlan specification verbatim, with
only surgical edits (de-branding, and replacing its proceed-to-the-next-
milestone instruction with stop-after-one), followed by an appended House
Rules section holding this repository's local deltas and an explicit clause
saying House Rules govern where the two overlap. Future changes are surgical
edits or House Rules additions, never a rewrite.

## Rationale

The first version of this file was written from scratch. It was shorter and
read well, but it was untested synthesis competing with prompt text that has
been used at scale; when the two were compared, everything the local version
had added — resolve ambiguity in the plan, prose over checklists, exact
commands with expected output, interfaces and recovery as required sections,
spike milestones — turned out to be present in the adopted text in stronger
wording. Only three deltas survived as genuinely local: context hygiene,
evidence discipline, and the active/completed lifecycle, which is what House
Rules now holds.

The accepted cost is length. The adopted document is longer than this
repository would have written and includes formatting guidance aimed at
plans pasted into a chat rather than committed as files. That guidance is
self-resolving for file-based plans, and editing it out would have meant
maintaining a fork of a document whose value is that it is not forked.

## Date

2026-09-16, v1 blueprint design conversation, at the user's direction.

## Status

accepted
