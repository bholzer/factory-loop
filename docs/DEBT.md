# Debt

## Register

| ID | Item | Where | Why deferred | Trigger to pay it down |
| --- | --- | --- | --- | --- |
| D1 | Template ↔ live correspondence is checked by hand | `template/` against the repository root | The live tree only came into existence with the correspondence rule, and writing the checker in the same session would have traded the milestone's content for its enforcement | The first commit that changes a `template/` file without its live counterpart, or the reverse |
| D2 | The scaffolding check cannot tell a quoted marker from a real slot | the second command under Commands in `AGENTS.md` | No file in the checked set needs to quote a marker, so keeping the marker definition to one owner is currently enough | The first live artifact in the checked set that has to quote a marker in its prose |

## Details

### D1 — Hand-checked template ↔ live correspondence

`ARCHITECTURE.md` states that every file under `template/` has a live
counterpart at the same relative path from the repository root, with identical
headings in identical order, and that `template/plans/PLANS.md` is
byte-identical to `plans/PLANS.md`. Nothing enforces either claim. Both were
established by hand: the file set discovered with `find` under `template/`,
byte identity with `cmp`, and structure by comparing headings.

Naive heading comparison does not work, because some headings are themselves
content: `ARCHITECTURE.md` names its components in headings, `docs/PRINCIPLES.md`
its principles, `docs/DEBT.md` its items, and every file's title. The rule that
does work, and that a mechanism should implement, is a subsequence test: take
the template file's headings, drop the ones containing a fill slot, and require
the remainder to appear in the live file in the same order. Extra live headings
are expected — filled content adds them.

Fixed would be one command that discovers the file set from `template/` itself,
applies byte identity where it is required and the subsequence test everywhere
else, and names the offending path and the missing or reordered heading in its
output. Two things such a command still would not catch: whether a live file's
content is actually about the same subject as its template counterpart, and
whether a live-only file ought to have had a template counterpart at all.

### D2 — Marker mention versus marker use

The check that live artifacts carry no leftover authoring scaffolding is a
grep for the fill and guidance markers. It cannot distinguish a marker that is
a real unfilled slot from one quoted in prose, and its own command line
contains the pattern, so the pattern is written with bracketed final letters
(`{{FIL[L]`) to keep it from matching the file it is listed in.

That trick handles self-matching but not genuine mentions. Two live files
originally tripped the check by discussing the convention; one was the command
line itself, and the other duplicated a definition that `template/AGENTS.md`
already owns, so removing the duplication fixed the check and the duplication
at once. The check therefore holds only while no file in its set needs to
quote a marker. Fixed would mean matching the markers' real shapes — a slot is
a brace pair opening a line or following whitespace outside backticks, and
guidance is a marker word opening an HTML comment block — rather than matching
the words anywhere.
