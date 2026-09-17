# 0021 — A path that must not resolve is written unbackticked or inside a quoted block, never excused by an allowlist

## Decision

When an artifact has to name a file that does not exist and should not — the
offending file in a failing-case instruction, the file a remediation transcript
shows being created, an illustrative name in an example — the path goes inside
an indented or fenced block, or into prose with no backticks around it.
Backticks mark a citation a reader is meant to be able to follow. An allowlist
entry is not the remedy for a path written the other way.

## Rationale

Two checks found this from opposite directions while they were being built. The
reference checker reads prose and skips quoted material, so a card whose
instruction cited its own must-not-exist example reported itself twice on the
run after it landed. The dependency checker deliberately reads quoted material,
because a dangling path inside a payload transcript still dangles for whoever
reads the copy, and it picked up a compound allowlist key that a card quotes as
a reference whose parent directory is a filename.

In both cases the alternative was an allowlist entry whose only content is that
a card is a card. That excuses the specification rather than a mention, and it
puts the exception in the one file nobody reads twice. Writing the path
unbackticked costs nothing, and the syntax then carries the meaning: a
backticked path is a claim that the target is there.

The cost is a convention an author has to know, which is why it is recorded
here instead of surviving as a house style in whichever file happened to get it
right. `docs/capabilities/CARD_FORMAT.md` requires every card to publish the
exact text of its failure, so a card is the artifact most likely to need this
rule, and the most expensive place to get it wrong.

## Date

2026-09-17, graduated from the plan that built this repository's gate set.

## Status

accepted
