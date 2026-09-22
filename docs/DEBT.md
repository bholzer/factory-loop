# Debt

## Register

| ID | Item | Where | Why deferred | Trigger to pay it down |
| --- | --- | --- | --- | --- |
| D2 | The scaffolding check cannot tell a quoted marker from a real slot | `tools/checks/scaffolding-markers` | No file in the checked set needs to quote a marker, so keeping the marker definition to one owner is currently enough | The first live artifact in the checked set that has to quote a marker in its prose |
| D6 | `boundary-lint` has no retrofit path for an existing codebase | `template/docs/capabilities/boundary-lint.md` against a brownfield target | Greenfield-first was the v1 scope choice in `GOALS.md`; a codebase that already violates its own layer map needs a baseline-and-ratchet story that no card here carries | The first bootstrap of this payload into a codebase whose declared layer map is already violated |
| D8 | The only gate is skippable, and absent in a fresh clone | `tools/hooks/pre-commit` against a clone that has not run the install line | Overdue rather than deferred: the remote arrived and runs nothing, so the hook is still the only gate that exists, and putting the cheap command somewhere the committer does not control is a build task rather than a correction | Fired — `git remote -v` names `origin` and `main` has been pushed to it; what remains is a job on that remote, which is a plan's worth of work |
| D9 | Two of the correspondence questions have no mechanism and stay a reading job | `docs/capabilities/template-live-drift.md`, read during the doc-garden pass | Neither question is decidable from text, so there is nothing to build: a heading list cannot tell whether two files discuss the same subject, and no comparison of what the payload ships can reveal what it failed to ship | A pair found structurally corresponding while saying different things, or a live-only artifact that a target project would have needed the payload to carry |
| D10 | One clause of the layer map — a skill body may not name a harness-specific tool — has no mechanism | `docs/capabilities/boundary-lint.md`, read during review | Deciding it needs a list of every tool name in every harness, which nobody can write and which the next harness release would invalidate | A skill body found naming a harness-specific tool, or a harness whose tool vocabulary is small and stable enough to enumerate |
| D11 | The two format documents are compared by heading subsequence though their class makes them one file stored twice | `tools/checks/template-live-drift`, `docs/capabilities/template-live-drift.md` | Widening the invariant of a card that already reads `enforced` requires the failing-case demonstration its own format asks for, which is a capability pass rather than a close-out edit; both pairs match today, so the gap is latent rather than active | The first wording difference between either pair of copies, or the next pass that opens that card for another reason |
| D13 | The live-trial card's failing case is one every harness routes around | `docs/capabilities/blueprint-eval.md` | The breakage the card specifies was demonstrated and the harness bootstrapped the copy regardless, by reading the tree; designing one that bites needs an observation nobody has made yet, and the five passing observations it was paired with are now recorded for all three harnesses | A session watched following a procedure its harness handed it at startup, rather than one it found by reading the files |
| D15 | The commit shape a driven run requires is written down only where a run has already been driven | `skills/plan-execute/SKILL.md` against `docs/capabilities/loop-runner.md` | The loop driver holds every commit of an iteration to touching the plan, while the execution procedure asks for a commit at each coherent step, so a session that lands its work and then its record stops a run that was otherwise going fine; settling it means editing an arriving procedure, and `docs/decisions/0025-a-payload-fix-mid-trial-invalidates-the-harness-results-before-it.md` prices that in retired trial evidence | A second project driven by the loop, or the next pass that opens the execution procedure for another reason |
| D16 | Step 9's install-nothing branch turns on locations a session cannot observe from where it runs | `skills/harness-init/SKILL.md`, step 9 | The candidate repair rewords a clause of an arriving procedure, which re-arms the trial gate and, under `docs/decisions/0025-a-payload-fix-mid-trial-invalidates-the-harness-results-before-it.md`, retires the three bootstrap records driven on 2026-09-22; it also needs an owner ruling on what a session can be asked to decide about its own harness | The next gated pass that opens the bootstrap procedure, or the next live-trial round — `D17` is priced by the same re-runs and pays down in the same round |
| D17 | The provenance line's phrasing rule leans on a citation convention the target never receives | `skills/harness-init/SKILL.md`, step 2 | The repair is one clause, but it lands in the same gated procedure `D16` names, and spending three re-driven bootstraps on a backtick alone buys almost nothing | The gated round that pays `D16` |

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

The trigger has since fired, which is why this row now reads overdue. `git
remote -v` names a fetch and push remote, and `git branch -avv` shows the
local branch tracking its counterpart there, so the place to run a check the
committer does not control now exists. Nothing runs there: the repository
carries no job configuration of any kind. Until one does, the paragraph above
stands unchanged and the row stays open with its reason changed from "there is
nowhere to run it" to "nobody has wired it up yet".

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

The first pass to do the reading caught three, all in one shape: a live card
stating something about this project's own register that the tree contradicts
— that a card had not been instantiated when it had, and twice that the
contract a plan is held to lives in the payload's card rather than in the
live one beside it. A heading walk cannot see any of them, and the reference
checker resolves the payload path happily, because the file it names does
exist. Two thirds of that is mechanizable and the row now says so: a live
card citing `template/docs/capabilities/<name>.md` where
`docs/capabilities/<name>.md` exists is a misrouting decidable from two path
tests. What stays a reading job is the other third — whether a claim about
instantiation state is still true — and that is why the row stands rather
than becoming a check today.

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

### D13 — A failing case no harness has been able to fall into

`docs/capabilities/blueprint-eval.md` asks for five observations per supported
harness and a demonstrated failing case before its status may move. The five
are now in hand for all three, against the payload as it currently stands:
each harness took an empty repository to a small command-line tool that its
own published verification command tests, in twenty to thirty minutes of
driven session time, and the plan that ran those trials carries the
transcript evidence per harness. The failing case is the half that remains.

The case the card specifies is to rename a procedure's entry-point file and
watch a harness fail to find it. That was run. The harness bootstrapped the
repository anyway: it listed the files, read the renamed one, and followed it,
because a bootstrap session arrives before any harness configuration exists in
the copy, so there is no loader in the path to break. The case therefore tests
the driver, which does report the wrong layout, and tests nothing about the
harness.

What the location decision changed is the shape of the missing observation,
which is why this row now names a narrower trigger than the one it carried
before. In two of the three harnesses the bootstrap installs a link, and a
probe run in the bootstrapped copy watched those harnesses offer all six
procedures to the next session from their own loader — so a loader in the path
is no longer hypothetical. What nobody has watched is a session depending on
one: reaching a procedure it would not otherwise have found. Only in that
situation does damaging the arriving set remove something a session was
actually using, and only then can a breakage be designed that a harness cannot
read its way around. Until someone observes it, the harness-side promotion bar
is unmeetable, and recording a status above `specced` would be claiming a gate
nobody has seen close.

### D16 — A branch condition decided from silence

Step 9 of `skills/harness-init/SKILL.md` installs nothing for procedure
reachability when every location the running harness loads procedures from
sits outside the repository. The 2026-09-22 re-runs watched a session take
that branch in the one harness whose earlier bootstraps had both gone the
other way: the skills it was handed at startup all came from machine-level
directories, the in-repository location the same harness had accepted
links under before was invisible because nothing was there to load, and
the session concluded the branch applied. Whatever the right answer was,
startup evidence could not have supplied it — it shows where the loaded
skills came from, not where a harness is willing to look.
`docs/specs/bootstrap-flow.md` records the run and the probe that
corroborated it.

The rule itself is not the defect: decision 0026 keeps the map's path
route working whichever branch fires. The open
question for the owners is whether the branch should be reworded to turn
on something a session can decide from where it stands, or whether
run-to-run variance between a committed link and nothing at all is an
acceptable cost of the shorter sentence. Either ruling edits an arriving
procedure, so it waits for the round the register prices it into.

### D17 — A phrasing rule whose protection stays behind

The optional provenance question in step 2 of
`skills/harness-init/SKILL.md` routes a named upstream into the target's
`GOALS.md` under scope, worded as a location outside that repository so
that no reference walk run there is ever pointed at it. What makes that
wording protective here is a convention of this repository's own —
`docs/decisions/0021-a-path-that-must-not-resolve-is-not-backticked.md`,
under which backticks mark the citations a checker chases — and nothing
the target receives states it. On 2026-09-22 one of the two sessions
handed an upstream answer recorded it backticked, satisfying every clause
the procedure states, and the trial driver's reference walk flagged both
mentions; the other session's unbackticked paragraph drew no flag.

The candidate repair is one clause in the routing sentence making the
unbackticked form explicit, and it waits beside `D16` for the next gated
round rather than re-arming the trial gate by itself.
