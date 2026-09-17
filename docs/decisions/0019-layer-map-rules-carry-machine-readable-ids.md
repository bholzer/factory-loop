# 0019 — The layer map's decidable rules carry identifiers, and the dependency check binds to them

## Decision

Every dependency rule in `ARCHITECTURE.md` that a machine can decide is written
twice in that file: as the paragraph that says why it exists, and as one
indented line in a block of identifiers giving the rule's id, its scope, and its
ban. `tools/checks/boundary-lint` binds to those identifiers and stops with an
undecided exit — not a pass — when the set in the map and the set it implements
differ in either direction. A rule the check cannot decide carries no id and is
stated as a review job with a row in `docs/DEBT.md`.

## Rationale

The check has to fail rather than pass when it cannot find the rules to apply,
because a run over zero rules is green output that means nothing. Prose is not
parsable: the map's own allowed-target sentence names a class of files in a
phrase no parser decides.

Fingerprinting the section's text was the alternative. It breaks on every
wording edit, and a check that fails on a typo teaches contributors to re-bless
the fingerprint without reading what changed — the worst possible habit for a
file that states dependency direction. Putting the rules only in the script was
the other alternative, and it moves the owner of a rule out of the document
whose subject is the architecture.

Both drift directions are loud on purpose. An id in the map with no
implementation is a rule nobody enforces; an implementation with no id is a rule
nobody wrote down. The cost is that adding a rule is two edits in one commit,
which is the property that keeps the map and the check describing the same
repository.

## Date

2026-09-17, graduated from the plan that built this repository's gate set.

## Status

accepted
