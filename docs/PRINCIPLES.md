# Principles

## The payload owes nothing to its home

The rule: nothing under `template/` may name a path outside `template/` or
mention this repository, because a target project receives those contents with
no ancestor context. `ARCHITECTURE.md`'s layer map owns the exact scope of the
restriction and the reason it runs in one direction only.
Violated when: a skeleton refers to the blueprint, to `skills/`, or to a file
that only resolves when read from this repository's root.

## One owner per fact

The rule: every fact lives in exactly one file, and every other file that
needs it links to that file instead of restating it. Where a restatement is
forced — a capability card must state the invariant it checks, a format
document the shape it defines — it names the owning file in the same section,
so the copy reads as attributed rather than competing. Nothing else earns one.
Violated when: two files state the same thing and one of them has quietly
gone stale — a capability status written on a card as well as in
`docs/capabilities/index.md`, or a rule copied out of `plans/PLANS.md` into a
skill body.

## Delete what has no reader left

The rule: content whose audience is gone is removed rather than annotated —
authoring guidance once a skeleton is filled, a debt row once the item is
paid, prose once it is wrong. Decision records are the single exception:
they are append-only, and a superseded record keeps its original text.
Violated when: a file accumulates struck-through, "(deprecated)", or
"no longer applies" content that every future reader still pays to read.

## Size is a context tax

The rule: every line in a file that is read at the start of a session must
earn its place against the cost of being re-read forever; `AGENTS.md` stays a
map under about 100 lines, and anything that wants to grow moves into `docs/`
behind a one-line pointer.
Violated when: a session-entry file grows a section that only some sessions
need, or a convention is padded with repetition for emphasis.
