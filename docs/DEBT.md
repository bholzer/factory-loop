# Debt

## Register

| ID | Item | Where | Why deferred | Trigger to pay it down |
| --- | --- | --- | --- | --- |
| D1 | Template ↔ live correspondence is checked by hand | `template/` against the repository root | The live tree only came into existence with the correspondence rule, and writing the checker in the same session would have traded the milestone's content for its enforcement | The first commit that changes a `template/` file without its live counterpart, or the reverse |
| D2 | The scaffolding check cannot tell a quoted marker from a real slot | the second command under Commands in `AGENTS.md` | No file in the checked set needs to quote a marker, so keeping the marker definition to one owner is currently enough | The first live artifact in the checked set that has to quote a marker in its prose |
| D3 | The payload's generic capability cards are not instantiated for this project | `docs/capabilities/` against `template/docs/capabilities/` | Writing the starter set plus this project's own three cards was one unit of work; instantiating four more registers here would have doubled it while nothing enforces any card in either half | The first of: a generic card reaching `built` anywhere, or a change landing here that one of the four applicable cards would have caught |
| D4 | The installed procedures sit outside every harness's auto-discovery root | `skills/` against a harness's own skills location | The file shape is portable and `AGENTS.md`'s map makes each procedure reachable by path, so an agent can always read one; only automatic surfacing is missing, and where to put the files is a per-harness configuration question that a live trial answers better than a guess | The first harness whose configuration cannot reach `skills/`, or the live trial specced in `docs/capabilities/blueprint-eval.md` |

## Details

### D1 — Hand-checked template ↔ live correspondence

`ARCHITECTURE.md` states that every file under `template/` has a live
counterpart at the same relative path from the repository root, that
`template/plans/PLANS.md` is byte-identical to `plans/PLANS.md`, and that
every other pair corresponds by heading subsequence. Nothing enforces any of
it. All three were established by hand: the file set discovered with `find`
under `template/`, byte identity with `cmp`, counterpart existence with
`test -e`, and structure by comparing headings.

What a mechanism must do is specified in
`docs/capabilities/template-live-drift.md`, including the exclusions a naive
walk gets wrong and the failing cases that promote it to `built`. This row is
the debt — the check does not exist — not a second copy of its design.

Two things such a command still would not catch, and which stay a reading
job after it is built: whether a live file's content is actually about the
same subject as its template counterpart, and whether a live-only file ought
to have had a template counterpart at all.

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

### D3 — Generic cards not instantiated here

The payload ships five cards in `template/docs/capabilities/`. Four state
invariants that hold for this repository too: `fast-verify` (the cheap
command), `evidence-check` (active plans carry their living sections),
`doc-integrity` (references resolve), and `boundary-lint` (the layer map in
`ARCHITECTURE.md`, whose rules are the outward-reference bans). The fifth,
`isolated-env`, does not apply: there is no toolchain and no runtime here, so
a card for it would be a check that passes on everything.

Each of the four invariants currently lives here as prose that a human
enforces by reading: the two commands under Commands in `AGENTS.md`, the
living-section requirements in `plans/PLANS.md`, and the first two entries of
`docs/PRINCIPLES.md` with the layer map in `ARCHITECTURE.md`. Paying this down
means copying each applicable card into `docs/capabilities/`, adding its row
to the register there with status `specced`, and adding its gating row to the
table in `docs/MATURITY.md` — after which this project's L1 gate set matches
the ladder's intent instead of being one card wide.

### D4 — Installed procedures are not auto-discovered

`docs/decisions/0008-portability-lowest-common-denominator.md` places skills
at `skills/<name>/SKILL.md` as the intersection of the three harnesses'
conventions. The file shape is genuinely the intersection — one directory per
skill, one `SKILL.md`, `name` matching the directory, a one-line
`description` — but the location is not. A harness that loads procedures
automatically reads them from a root it chooses itself, not from a
repository-root `skills/`: one of the three scans an ancestor dotted
directory for `skills/*/SKILL.md` and reaches anywhere else only through a
configured extra directory. So the six procedures here are readable by path
and listed in `AGENTS.md`'s map, and that is the whole mechanism today.

Three ways to pay it down, and choosing between them wants evidence from a
real session rather than reasoning: configure each harness to scan `skills/`;
move the canonical location into whichever dotted directory the harnesses
share and keep the map pointing at it; or have the bootstrap procedure place
a link into the environment's own root, which is what its step 9 already
tells it to do. The first two would change what the payload installs, so both
are decisions, not fixes.
