# 0005 — The template's copy of the plan convention is byte-identical to the live one

## Decision

`template/plans/PLANS.md` and `plans/PLANS.md` are byte-identical: `cmp`
between them must be silent. Not a symlink, not an include directive, not an
annotated variant.

## Rationale

The payload must survive a plain recursive copy onto any filesystem, so
symlinks are out, and no skill may depend on symlink support. Identity also
makes this the one correspondence check that requires no judgement: every
other template/live pair varies by project-specific content, and this pair
varies not at all, which makes it the simplest possible drift test.

The cost is that this file cannot carry the ownership-boundary header that
every other template file carries, because adding one would break identity.
That trade is accepted: a header helps a reader once, while exactness helps
every future drift check. Generalization turned out to be unnecessary anyway
— the convention's text refers only to "this repository" and to paths the
payload itself provides.

## Date

2026-09-16, v1 blueprint design conversation.

## Status

accepted
