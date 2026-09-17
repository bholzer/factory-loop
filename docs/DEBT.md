# Debt

## Register

| ID | Item | Where | Why deferred | Trigger to pay it down |
| --- | --- | --- | --- | --- |
| D2 | The scaffolding check cannot tell a quoted marker from a real slot | `tools/checks/scaffolding-markers` | No file in the checked set needs to quote a marker, so keeping the marker definition to one owner is currently enough | The first live artifact in the checked set that has to quote a marker in its prose |
| D3 | The payload's generic capability cards are not instantiated for this project | `docs/capabilities/` against `template/docs/capabilities/` | Writing the starter set plus this project's own three cards was one unit of work; instantiating four more registers here would have doubled it while nothing enforces any card in either half | Fired at v1 close: the reference sweep found a dangling reference that `doc-integrity` decides. Overdue — see Details |
| D4 | The installed procedures sit outside every harness's auto-discovery root | `skills/` against a harness's own skills location | The file shape is portable and `AGENTS.md`'s map makes each procedure reachable by path, so an agent can always read one; only automatic surfacing is missing, and where to put the files is a per-harness configuration question that a live trial answers better than a guess | The first harness whose configuration cannot reach `skills/`, or the live trial specced in `docs/capabilities/blueprint-eval.md` |
| D5 | The unattended outer loop is specced, not built | `docs/capabilities/loop-runner.md` | v1 is the watched phase: a human invokes each milestone session and judges after each whether iteration continues, which is how the failure domains get seen before they are automated away | Enough consecutive sessions whose stop rule held without human correction that the halt conditions are known, or L2 being wanted for another reason |
| D6 | `boundary-lint` has no retrofit path for an existing codebase | `template/docs/capabilities/boundary-lint.md` against a brownfield target | Greenfield-first was the v1 scope choice in `GOALS.md`; a codebase that already violates its own layer map needs a baseline-and-ratchet story that no card here carries | The first bootstrap of this payload into a codebase whose declared layer map is already violated |
| D7 | The blueprint has never been run; evaluation is paper-only | `docs/capabilities/blueprint-eval.md` | The owners scoped v1 to paper verification. A trial needs a target project, two or more harnesses, and one feature driven through the loop end to end — its own unit of work, not a milestone tail | Any of: a project bootstrapped from this payload for real, a harness whose configuration cannot reach `skills/` (`D4`), or a payload change whose effect reading cannot predict |
| D8 | The only gate is skippable, and absent in a fresh clone | `tools/hooks/pre-commit` against a clone that has not run the install line | A hook is the strongest enforcement point a repository with no remote has; making it unskippable needs a place to run that the committer does not control, and there is none yet | The first remote or continuous-integration system this repository gets |
| D9 | Two of the correspondence questions have no mechanism and stay a reading job | `docs/capabilities/template-live-drift.md`, read during the doc-garden pass | Neither question is decidable from text, so there is nothing to build: a heading list cannot tell whether two files discuss the same subject, and no comparison of what the payload ships can reveal what it failed to ship | A pair found structurally corresponding while saying different things, or a live-only artifact that a target project would have needed the payload to carry |

## Details

### D2 — Marker mention versus marker use

The check that live artifacts carry no leftover authoring scaffolding is
`tools/checks/scaffolding-markers`, a grep for the fill and guidance markers
over eight named artifacts. It cannot distinguish a marker that is a real
unfilled slot from one quoted in prose. The pattern lives in that script and
is written with bracketed final letters (`{{FIL[L]`) so that a file which only
mentions the markers — this register among them — does not trip it.

That trick handles self-matching but not genuine mentions. Two live files
originally tripped the check by discussing the convention; one was the check's
own published command line, which is now the script body, and the other
duplicated a definition that `template/AGENTS.md` already owns, so removing
the duplication fixed the check and the duplication at once. The check
therefore holds only while no file in its set needs to quote a marker. Fixed
would mean matching the markers' real shapes — a slot is a brace pair opening
a line or following whitespace outside backticks, and guidance is a marker
word opening an HTML comment block — rather than matching the words anywhere.

### D3 — Generic cards not instantiated here

The payload ships six cards in `template/docs/capabilities/`. Five state
invariants that hold for this repository too: `fast-verify` (the cheap
command), `evidence-check` (active plans carry their living sections),
`doc-integrity` (references resolve), `boundary-lint` (the layer map in
`ARCHITECTURE.md`, whose rules are the outward-reference bans), and
`prose-duplication` (no run of prose in two artifacts). The sixth,
`isolated-env`, does not apply: there is no toolchain and no runtime here, so
a card for it would be a check that passes on everything.

Two of the five are now instantiated and enforced: `fast-verify` sits at
`docs/capabilities/fast-verify.md` and runs as `./tools/verify`, and
`doc-integrity` sits at `docs/capabilities/doc-integrity.md` and runs as
`tools/checks/doc-integrity` under it. The other three still live here as
prose that a human enforces by reading — the living-section requirements in
`plans/PLANS.md`, and the first two entries of `docs/PRINCIPLES.md` with the
layer map in `ARCHITECTURE.md`. Paying the rest down means copying each
remaining card into `docs/capabilities/`, adding its row to the register
there, and adding a gating row to the table in `docs/MATURITY.md` for each
card that gates a rung — after which this project's L1 gate set matches the
ladder's intent instead of being one card wide.

The trigger fired at the close of v1, on both of its clauses. The reference
sweep, run by hand, found decision record `0015` citing `MATURITY.md` — a path
that resolves from neither the repository root nor that record's own
directory, which is exactly the miss `doc-integrity` decides. The duplication
sweep, also by hand, found twelve pairs of live artifacts sharing eight-word
windows; eight were real single-owner violations and were fixed, which is what
`prose-duplication` decides. Both defects had been in the tree for at least
one milestone before anyone looked. The row stands as overdue, and the copying
described above is a plan of its own rather than a tail on a milestone.

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

### D6 — No brownfield path for a dependency check

`template/docs/capabilities/boundary-lint.md` specifies a check that refuses a
dependency edge the layer map forbids, and it assumes a tree where no such
edge exists yet. Bootstrapping into an old codebase inverts that: the map is
written by reading what is there, so the first run reports hundreds of real
violations and the only options are to switch the check off or to weaken the
map until it describes the mess.

What is missing is the third option, and it is a card change rather than a
build: a recorded baseline of the edges that exist on the day the map is
written, a refusal that applies only to edges absent from that baseline, and
a count that may go down but never up. Two things need deciding with a real
codebase in front of you — where the baseline lives so that deleting a line
from it is a visible commit, and whether the ratchet is enforced per file or
per repository. Until one exists, the card's acceptance is unreachable on a
brownfield target, and D3's copying pass must not pretend otherwise.

### D8 — A gate the committer can turn off

`tools/hooks/pre-commit` runs `./tools/verify` and refuses the commit when it
fails, but three things weaken it. `git commit --no-verify` skips it and
leaves no trace that it was skipped. A fresh clone has no hook at all until
someone runs `git config core.hooksPath tools/hooks`, which `AGENTS.md`
publishes but nothing enforces. And the hook judges the working tree rather
than the staged content, so committing a subset of a dirty tree is decided on
the whole tree — conservative in the direction that refuses too much, which is
why it is a weakness and not a hole.

The consequence that matters is for `docs/MATURITY.md`: its promotion rule
requires a gating card to be `enforced` with no run in which the check was
disabled or skipped, and a flag that silently turns the gate off makes that
run unobservable. No rung may be claimed on this gate until the check also
runs somewhere the committer does not control.

### D9 — The part of correspondence a heading list cannot see

`tools/checks/template-live-drift` now decides the three comparisons its card
states, and the card's Invariant section names the two questions it leaves
open. Neither is a missing implementation. The first asks about meaning, and
the check reads only structure; the second asks about an absence, and the
check enumerates the payload rather than judging it.

So the owner is a person: `skills/doc-garden/SKILL.md` is the pass where the
two halves get read against each other, and this row exists so that the pass
has a written reason to do it rather than trusting the green check to mean
more than it does. Paying it down is not writing a checker. It is either
finding a case the reading catches — which is evidence for what a mechanism
would have to decide — or concluding that the reading has caught nothing over
enough passes to retire the row.
