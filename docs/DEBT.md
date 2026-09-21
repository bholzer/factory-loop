# Debt

## Register

| ID | Item | Where | Why deferred | Trigger to pay it down |
| --- | --- | --- | --- | --- |
| D2 | The scaffolding check cannot tell a quoted marker from a real slot | `tools/checks/scaffolding-markers` | No file in the checked set needs to quote a marker, so keeping the marker definition to one owner is currently enough | The first live artifact in the checked set that has to quote a marker in its prose |
| D4 | The installed procedures sit outside every harness's auto-discovery root | `skills/` against a harness's own skills location | Three live trials found the shortfall costs discovery latency and not access: no harness offered the arriving procedures at startup, and every bootstrap session reached the right one within its first two commands by reading the tree | The pick between the three candidate locations described below, which is a decision rather than a fix and no longer waits on anything to be observed |
| D5 | The unattended outer loop is specced, not built | `docs/capabilities/loop-runner.md` | v1 is the watched phase: a human invokes each milestone session and judges after each whether iteration continues, which is how the failure domains get seen before they are automated away | Enough consecutive sessions whose stop rule held without human correction that the halt conditions are known, or L2 being wanted for another reason |
| D6 | `boundary-lint` has no retrofit path for an existing codebase | `template/docs/capabilities/boundary-lint.md` against a brownfield target | Greenfield-first was the v1 scope choice in `GOALS.md`; a codebase that already violates its own layer map needs a baseline-and-ratchet story that no card here carries | The first bootstrap of this payload into a codebase whose declared layer map is already violated |
| D8 | The only gate is skippable, and absent in a fresh clone | `tools/hooks/pre-commit` against a clone that has not run the install line | A hook is the strongest enforcement point a repository with no remote has; making it unskippable needs a place to run that the committer does not control, and there is none yet | The first remote or continuous-integration system this repository gets |
| D9 | Two of the correspondence questions have no mechanism and stay a reading job | `docs/capabilities/template-live-drift.md`, read during the doc-garden pass | Neither question is decidable from text, so there is nothing to build: a heading list cannot tell whether two files discuss the same subject, and no comparison of what the payload ships can reveal what it failed to ship | A pair found structurally corresponding while saying different things, or a live-only artifact that a target project would have needed the payload to carry |
| D10 | One clause of the layer map — a skill body may not name a harness-specific tool — has no mechanism | `docs/capabilities/boundary-lint.md`, read during review | Deciding it needs a list of every tool name in every harness, which nobody can write and which the next harness release would invalidate | A skill body found naming a harness-specific tool, or a harness whose tool vocabulary is small and stable enough to enumerate |
| D11 | The two format documents are compared by heading subsequence though their class makes them one file stored twice | `tools/checks/template-live-drift`, `docs/capabilities/template-live-drift.md` | Widening the invariant of a card that already reads `enforced` requires the failing-case demonstration its own format asks for, which is a capability pass rather than a close-out edit; both pairs match today, so the gap is latent rather than active | The first wording difference between either pair of copies, or the next pass that opens that card for another reason |
| D12 | An expected command line in a plan is never parsed before the session that has to run it | `skills/plan-author/SKILL.md`, step 8 | The repair is one sentence in an arriving procedure, and editing a procedure invalidates every recorded result that judged a session following it, so the sentence costs a re-driven session in each supported harness and wants a pass that is driving them anyway | The next pass that edits the authoring procedure for another reason, or the next run of the live trial |
| D13 | The live-trial card has no promotion evidence: one harness has carried a feature through the payload as it stands, and the breakage that should catch a broken procedure set catches nothing | `docs/capabilities/blueprint-eval.md` | Both halves cost driven sessions rather than edits, and the second half is a design nobody has an observation for — every harness watched so far reached the procedures by reading the files, so a wrong entry-point filename removed nothing any of them was using | Any change under `template/` or `skills/`, which this card already gates, or any attempt to record a status above `specced` for it |

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
configured extra directory. Three live trials have now measured that, and the
sentence understates it: none of the three harnesses offered the arriving
procedures to a session at startup.

Each trial copied the payload into a fresh repository outside this tree, drove
a bootstrap session in it, and — in two of the three — asked a separate
read-only session to name every procedure it had been handed and where each
one came from. What came back was the machine owner's own roots: twenty names
in one harness, twenty-four in the other, and none of the six that had just
arrived. Every bootstrap session found the right procedure regardless, inside
its first two commands, by listing the files and reading one. That is the map
mechanism working, minutes before any harness configuration existed in the
copy at all.

The link the bootstrap procedure already places is the one candidate with a
controlled measurement behind it. In the harness probed on both sides, the
bootstrapped copy carrying a link into the location that harness reads named
all six at startup, where the same copy before the link named none. In a
second harness the link was written and never probed. In the third the
session installed nothing at all and the copy finished the trial with no
harness configuration of any kind, because the roots that harness reads are
the machine owner's directories rather than anywhere inside a project, so
writing one would configure the machine instead of the repository.

The three candidates therefore stand unchanged and the choice between them is
now informed rather than speculative: configure each harness to scan
`skills/`; move the canonical location into whichever dotted directory the
harnesses share and keep the map pointing at it; or keep the per-environment
link, which ships already and is the only candidate anyone has watched work.
The first two change what the payload installs, so both are decisions, not
fixes. What is no longer missing is the evidence.

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

### D12 — A command line nobody parsed until it mattered

`skills/plan-author/SKILL.md` tells an author to reread the plan as the
session that will execute it, and a plan under `plans/PLANS.md` is required
to carry the exact commands a later session runs. Neither says that the
command itself has to be tried. One authoring pass here composed a driver
invocation for an external program out of flags each of which was read from
that program's own help output, and the composition was rejected by argument
parsing on first contact: two of the flags are mutually exclusive. A review
pass read the same line and confirmed it the same way, by recognising the
flags.

The candidate repair is one sentence: an expected invocation is run until the
first refusal the environment can produce without doing the work — argument
parsing, authentication, a version banner — and the point it reached is
written beside it. The reason it is a row and not an edit is the cost of
editing an arriving procedure. Any recorded result about a session that
followed that procedure describes the text as it stood, so a one-sentence
change retires three such records and buys them back only by driving three
more sessions. That is a pass of its own, and the trigger names the two
occasions that would be paying the cost anyway.

The generalisation is what makes the row worth keeping rather than merging
into the next plan's preamble. Three separate defects in one piece of work
shared a shape: a statement about how parts behave together, checked one part
at a time. The unparseable command line is the cheapest of the three to
prevent, which is why it is the one with a written remedy.

### D13 — A trial that ran, and a promotion that cannot follow yet

`docs/capabilities/blueprint-eval.md` asks for five observations per supported
harness and a demonstrated failing case before its status may move. The trial
has run in all three harnesses and neither condition is met.

The first half is bookkeeping. A defect the trial found was repaired partway
through, and only the first session of each affected run was driven again, so
two of the three records describe a payload that has since changed. Exactly one
harness has taken a feature from an empty repository to an executed milestone
against the current text. Paying it down is two runs of three sessions each,
roughly half an hour of unattended session time per harness, plus the reading.

The second half is a design problem and the reason this row exists rather than
a note in a goal. The case the card specifies is to rename a procedure's
entry-point file and watch a harness fail to find it. That was run. The harness
bootstrapped the repository anyway: it listed the files, read the renamed one,
and followed it, because no harness observed in the trial reached a procedure
through a loader in the first place. So the case tests the driver, which does
report the wrong layout, and tests nothing about the harness. A replacement has
to break something a session cannot route around by reading — and nobody has
watched a harness fall into such a breakage, which is exactly the kind of
unobserved clause the trial exists to catch. Until one exists, the card's
harness-side promotion bar is unmeetable, and recording a status above
`specced` would be claiming a gate nobody has seen close.
