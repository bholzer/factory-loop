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
states the rule, and the report a bootstrap ends with names which of the three
it took.

All three outcomes have been observed in the harnesses this project supports.
Claude Code and omp each committed a link — at .claude/skills and .omp/skills
respectively, neither of them a location the payload ships — pointing back at
the unmoved `skills/` directory; a probe in each bootstrapped copy then listed
all six procedures as loaded from the copy's own tree, where the same probe in
an unbootstrapped copy listed none of them. Codex CLI installed nothing, which
is correct there: the roots it reads are the machine owner's own directories,
so a project cannot reach them without configuring the machine.

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
an agent the owner answers a file cannot supply, and lets it work. The most
recent round ran once in each supported harness against the payload as it now
stands, three separate sessions each time — fill the artifacts, author a plan,
execute that plan's first milestone — and every run ended with a small
command-line tool that records labels and prints counts, a test suite behind a
command the project chose and published itself, and a plan whose later
milestones were left open. The three projects picked three different names for
their stored data and three different names for their verification command,
and answered identically at the terminal. None of the three sessions in any
run was handed the arriving procedures at startup; each found the bootstrap
procedure by listing the files and reading it, within its first two commands.

What the runs cost, as a sizing figure for anyone repeating it: 20, 22 and 29
minutes of driven session time per harness for the three sessions together,
unattended.

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
