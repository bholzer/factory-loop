# Debt

## Register

| ID | Item | Where | Why deferred | Trigger to pay it down |
| --- | --- | --- | --- | --- |
| D2 | The scaffolding check cannot tell a quoted marker from a real slot | `tools/checks/scaffolding-markers` | No file in the checked set needs to quote a marker, so keeping the marker definition to one owner is currently enough | The first live artifact in the checked set that has to quote a marker in its prose |
| D4 | The installed procedures sit outside every harness's auto-discovery root | `skills/` against a harness's own skills location | The file shape is portable and `AGENTS.md`'s map makes each procedure reachable by path, so an agent can always read one; only automatic surfacing is missing, and where to put the files is a per-harness configuration question that a live trial answers better than a guess | The first harness whose configuration cannot reach `skills/`, or the live trial specced in `docs/capabilities/blueprint-eval.md` |
| D5 | The unattended outer loop is specced, not built | `docs/capabilities/loop-runner.md` | v1 is the watched phase: a human invokes each milestone session and judges after each whether iteration continues, which is how the failure domains get seen before they are automated away | Enough consecutive sessions whose stop rule held without human correction that the halt conditions are known, or L2 being wanted for another reason |
| D6 | `boundary-lint` has no retrofit path for an existing codebase | `template/docs/capabilities/boundary-lint.md` against a brownfield target | Greenfield-first was the v1 scope choice in `GOALS.md`; a codebase that already violates its own layer map needs a baseline-and-ratchet story that no card here carries | The first bootstrap of this payload into a codebase whose declared layer map is already violated |
| D7 | The blueprint has never been run; evaluation is paper-only | `docs/capabilities/blueprint-eval.md` | The owners scoped v1 to paper verification. A trial needs a target project, two or more harnesses, and one feature driven through the loop end to end — its own unit of work, not a milestone tail | Any of: a project bootstrapped from this payload for real, a harness whose configuration cannot reach `skills/` (`D4`), or a payload change whose effect reading cannot predict |
| D8 | The only gate is skippable, and absent in a fresh clone | `tools/hooks/pre-commit` against a clone that has not run the install line | A hook is the strongest enforcement point a repository with no remote has; making it unskippable needs a place to run that the committer does not control, and there is none yet | The first remote or continuous-integration system this repository gets |
| D9 | Two of the correspondence questions have no mechanism and stay a reading job | `docs/capabilities/template-live-drift.md`, read during the doc-garden pass | Neither question is decidable from text, so there is nothing to build: a heading list cannot tell whether two files discuss the same subject, and no comparison of what the payload ships can reveal what it failed to ship | A pair found structurally corresponding while saying different things, or a live-only artifact that a target project would have needed the payload to carry |
| D10 | One clause of the layer map — a skill body may not name a harness-specific tool — has no mechanism | `docs/capabilities/boundary-lint.md`, read during review | Deciding it needs a list of every tool name in every harness, which nobody can write and which the next harness release would invalidate | A skill body found naming a harness-specific tool, or a harness whose tool vocabulary is small and stable enough to enumerate |
| D11 | The two format documents are compared by heading subsequence though their class makes them one file stored twice | `tools/checks/template-live-drift`, `docs/capabilities/template-live-drift.md` | Widening the invariant of a card that already reads `enforced` requires the failing-case demonstration its own format asks for, which is a capability pass rather than a close-out edit; both pairs match today, so the gap is latent rather than active | The first wording difference between either pair of copies, or the next pass that opens that card for another reason |

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
brownfield target, and instantiating it for a greenfield project must not
pretend otherwise.

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

### D10 — The layer-map clause a tool list cannot decide

`tools/checks/boundary-lint` decides three of the four rules in
`ARCHITECTURE.md`'s layer map, and `docs/capabilities/boundary-lint.md` states
the fourth as the limitation it leaves open: a skill body may not name a
harness-specific tool. Nothing in a path reference distinguishes a tool name
from an ordinary word, so the check would need an enumeration of every tool
name in every harness the procedures are meant to run in — a list nobody can
write completely and one that the next release of any harness invalidates.

The owner is therefore review, and specifically the moment a procedure is
edited: the reviewer asks whether a named command exists outside the harness
in front of them. The exposure is small because the rule bites only in six
files, all of them short, and none of them names a tool today. Paying it down
is either a harness whose tool vocabulary turns out to be stable enough to
enumerate, or the first body that breaks the rule, which would be evidence
about what an enumeration would have to contain.

### D11 — Two copies of a format, compared as if they could differ

Two files here are formats a project conforms to rather than skeletons it
fills: `docs/capabilities/CARD_FORMAT.md`, and the record format beside it at
`docs/decisions/DECISION_FORMAT.md`. Each live copy is the payload's copy
verbatim, which makes the pair one file stored twice. The
correspondence invariant in `ARCHITECTURE.md` names only the plan convention as
the pair that must agree to the byte, and `tools/checks/template-live-drift`
compares these two pairs the way it compares a filled skeleton, by heading
subsequence, so a sentence rewritten inside a section of one copy and left
alone in the other is invisible to it. Both pairs agree today: `cmp` printed
nothing on either pair when the plan that built the checks was closed out.

Paying it down is three lines and one demonstration. The `BYTE_IDENTICAL`
literal in the check takes the two paths, and the Invariant section of
`docs/capabilities/template-live-drift.md` and the correspondence bullet in
`ARCHITECTURE.md` each gain the clause that says why those pairs are held to
the stricter comparison. The demonstration is what makes this a pass of its
own rather than an edit made in passing: a card reading `enforced` whose
invariant widens has to be observed failing on a real difference first, which
`docs/capabilities/CARD_FORMAT.md` requires of any status claim.
