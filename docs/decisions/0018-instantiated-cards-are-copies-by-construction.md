# 0018 — A live card at a payload card's relative path is that card's instantiation, and the pair is exempt from the duplication check

## Decision

Where a live file sits at the same path below the repository root as a file
under `template/`, the two are copies of each other by construction, and
`ARCHITECTURE.md` owns that relationship. `tools/checks/prose-duplication`
skips every such pair instead of reporting the prose they share. A live card at
a payload card's relative path is the generic invariant rewritten for what this
project actually has, which is why the same pair stays outside the structural
correspondence comparison as well: an instantiation is not a fill.

## Rationale

The live half is the filled form of the payload, so the two copies of an agent
guide already share long runs of working-rule text before anyone instantiates
anything. Instantiating cards adds one such pair per card. The alternative was
an allowlist entry per pair, each one saying that a card is the card it
instantiates — an exception list that grows with every card and that is the
size of the thing it excuses. A declared correspondence is decidable from the
paths alone, so it belongs in the check as a class rather than in a file of
judgement calls.

`docs/decisions/0014-cards-are-project-content.md` took card files out of
counterpart-existence checking without saying what the relationship is when a
counterpart does exist. This record says it, which is what lets a reader tell an
instantiation from drift.

The cost is that duplication between a live card and its payload twin is not
decided by anything mechanical. That is the correct trade: the two files say the
same thing on purpose, and the question worth asking about them — whether the
live one still describes this project's real check — is a reading job that
`docs/capabilities/CARD_FORMAT.md` assigns to review.

## Date

2026-09-17, graduated from the plan that built this repository's gate set.

## Status

accepted
