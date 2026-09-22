# The bootstrap flow

## What this repository offers an outside reader

Two things, both installable by copying files. The payload in `template/` is
an artifact set a project receives and fills in: an agent guide, goals,
architecture, a principles file, a maturity ladder, a debt register, a
decision-record directory with its format, a behavior-spec directory with its
reflection rule, a capability register with six starter cards and their
format, and the plan convention. The procedures in `skills/` are six
markdown files an agent reads and follows: `harness-init`, `plan-author`,
`plan-execute`, `doc-garden`, `retro`, `capability-build`.

Nothing is executed to install either one. There is no generator, no
templating engine, and no dependency: the observable operation is a recursive
copy followed by an editing pass that replaces the payload's fill slots with
project content and deletes its authoring guidance.

## What the bootstrap asks the owners

The flow's one interview happens early and in a single pass, collecting the
facts no file in the target can supply: purpose, audience, what success
would look like, the deliberate exclusions, the non-goals worth writing
down, the constraints that outrank convenience, and the unknowns the owners
can already name. Step 2 of `skills/harness-init/SKILL.md` owns the
question list, along with the rule that an unanswered question is recorded
as an unknown rather than papered over with an invented fact.

Since 2026-09-22 the same pass carries one optional question: where the
installed artifact set and procedures came from, and where that upstream
keeps the guide it publishes for adopters. A named location becomes a
single statement in the target's own `GOALS.md`, under its scope section,
given as a location outside that repository — a URL or a checkout path —
which keeps any reference checker the target later builds from being
pointed at it. Silence writes nothing: an upstream nobody named is not
recorded.

Both branches of the optional question have been observed, one bootstrap
per supported harness on 2026-09-22. The two copies whose owners named an
upstream both hold the record where the rule routes it — in one copy as a
short unbackticked paragraph under the scope heading, in the other as one
sentence with the locations backticked, which the trial driver's reference
walk promptly chased; `docs/DEBT.md` `D17` carries that divergence. The
copy whose owners named no upstream mentions one nowhere, and its report
volunteered the decline unprompted. The reports are otherwise the weaker
audit surface: the session that wrote the fullest pointer never said so,
and only its tree shows the record.

## What a project has after the flow runs

An agent opening the project cold reads one file, `AGENTS.md`, and from its
map reaches every other artifact and the two or three commands the project
actually runs. Multi-file work goes through a plan under `plans/active/` that
a later session can execute from that file and the worktree alone. Every
invariant the project wants enforced exists as a card in
`docs/capabilities/`, with its status in the register beside it, and nothing
is enforced mechanically until someone builds one — the ladder in
`docs/MATURITY.md` starts at L0 and moves only by the rule stated there.

Each procedure is invoked by reading the file at its mapped path. That route
works in any agent environment and is the one the payload guarantees. On top
of it the bootstrap installs reachability for the harness in front of it,
without moving anything: the harness's configuration, when that configuration
is something the repository itself holds; a committed link, when the harness
reads an in-repository location the project does not ship; and nothing at all,
when every location it reads belongs to the machine rather than to any
project.
`docs/decisions/0026-installed-procedures-stay-put-and-reachability-is-installed-per-harness.md`
states the rule. Since the 2026-09-22 procedure edits, the report a
bootstrap ends with answers the two clauses of the procedure's step 9 —
entry points, then reachability — one at a time:
the entry-point file added or why none was needed; what reachability
was installed and where, or the ground for installing nothing, an outcome
`skills/harness-init/SKILL.md` now requires the report to state on its own
rather than leave to inference. All three re-run reports met that
obligation, no two in the same grammatical shape.

All three of the rule's outcomes have been observed in the harnesses this
project supports, most recently in the 2026-09-22 re-runs — one bootstrap
per harness against the current procedure text. Claude Code committed a
link at .claude/skills — not a location the payload ships — pointing back
at the unmoved `skills/` directory, and a probe in the bootstrapped copy
listed all six procedures as loaded from the copy's own tree through it.
Codex CLI installed nothing, which is correct there: the roots it reads
are the machine owner's own directories, so a project cannot reach them
without configuring the machine. omp installed nothing this time — its two
earlier recorded bootstraps had each committed a link, once at
.claude/skills and once at .omp/skills — and the probe run in its copy
found none of the six procedures in the next session's startup set,
leaving the map's path reference as that copy's route into them. Same
text, same harness version, three outcomes across three runs: the branch
turned on what the session could observe about the locations its
environment reads, and `docs/DEBT.md` `D16` carries the wording question
that observation raises.

## What holds today, and what has not been observed

Established by running the checks: the payload survives a plain recursive
copy into an empty directory with every reference inside the copy resolving,
and all six procedures reference only paths the copy provides. This
repository is itself the filled form of that payload, which is what makes
divergence between the two halves visible at all.

Established by running the flow somewhere else, most recently on 2026-09-21
and 2026-09-22: a different project can be bootstrapped from this payload, and
a feature can be driven through the plan loop with no access to this tree. A
trial builds a git repository outside this repository from both halves, gives
an agent the owner answers a file cannot supply, and lets it work. The
fullest round ran once in each supported harness, three separate sessions
each time — fill the artifacts, author a plan,
execute that plan's first milestone — and every run ended with a small
command-line tool that records labels and prints counts, a test suite behind a
command the project chose and published itself, and a plan whose later
milestones were left open. The three projects picked three different names for
their stored data and three different names for their verification command,
and answered identically at the terminal. None of the three sessions in any
run was handed the arriving procedures at startup; each found the bootstrap
procedure by listing the files and reading it, within its first two commands.

That round ran against the bootstrap procedure as it stood before the
2026-09-22 edits — the per-clause reporting rule and the optional
provenance question. The scoping in
`docs/decisions/0025-a-payload-fix-mid-trial-invalidates-the-harness-results-before-it.md`
retires only the records of sessions that followed the text an edit
changed, so that round's bootstrap-session records are superseded by the
same-day re-runs recorded above, one fresh bootstrap per harness against
the edited text, while its authoring and execution records stand: neither
of those procedures changed.

What the rounds cost, as sizing figures for anyone repeating them: 20, 22
and 29 minutes of driven session time per harness for the three sessions
together, unattended; the 2026-09-22 bootstrap re-runs alone took just
under six minutes in two of the harnesses and just over ten and a half in
the third.

Not established, and worth naming precisely because the rest now is. No
harness has been watched failing when the procedure set arrives damaged — the
one deliberately broken copy was bootstrapped anyway, because a bootstrap
session runs before any harness configuration exists in the copy and reaches
the procedures by reading the tree, so a wrong filename removed nothing it was
using. `docs/DEBT.md` `D13` names the observation a usable breakage waits on.
Every target was greenfield and every target was the same kind
of tool, so nothing here says how the payload lands on an existing codebase —
`docs/DEBT.md` `D6` owns that — or on a project shaped unlike a single-user
command-line program.
