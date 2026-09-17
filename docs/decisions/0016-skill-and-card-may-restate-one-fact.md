# 0016 — A portable procedure and a prunable card may state the same fact

## Decision

Where a procedure under `skills/` and a capability card must both state the
same fact, both statements stand. The procedure's copy governs the pass a
human or agent performs by hand; the card's copy governs the mechanism a
builder implements. Neither is deleted in favour of a pointer, and the pair is
carried as an allowlist entry in any duplication check rather than treated as
a defect to repair.

## Rationale

A pointer is unavailable in both directions. A procedure may not name a card
file, because cards are project content that a project prunes, so the
reference would dangle in exactly the projects that pruned it. Nothing in the
payload may name `skills/`, because a target project receives the payload
without the procedures in it. The two files are therefore mutually unnameable
by construction, and the one-owner rule's usual remedy does not apply.

Three ways out were available. A third shipped-verbatim document that both
could cite would be a new artifact whose entire content is two lists, created
to serve a rule rather than a reader — and the alternative to creating it is
one allowlist entry per pair. Dropping the statement from the procedure would
leave the pass undefined in any project that pruned the card, which is the
case the procedure most needs to survive. Dropping it from the card would make
the check unbuildable without reading a file the payload does not ship.

The exposure is small and measured rather than assumed: two pairs, eleven
shared eight-word windows — `skills/doc-garden/SKILL.md` against
`template/docs/capabilities/doc-integrity.md`, where both carry the classes of
legitimately unresolvable reference, and `skills/harness-init/SKILL.md` against
`template/docs/capabilities/isolated-env.md`, where both say what deleting a
card entails. Both facts are load-bearing in both places.

What this does not license is a procedure restating a rule from a file it
*can* name. `docs/PRINCIPLES.md` requires a forced restatement to name its
owner in the same section, and every restatement that can be attributed still
must be.

## Date

2026-09-17, v1 blueprint close.

## Status

accepted
