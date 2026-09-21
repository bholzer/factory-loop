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

Each procedure is invoked by reading the file at its mapped path. That works
in any agent environment; automatic surfacing does not, and `docs/DEBT.md`
`D4` carries why.

## What holds today, and what has not been observed

Established by running the checks: the payload survives a plain recursive
copy into an empty directory with every reference inside the copy resolving,
and all six procedures reference only paths the copy provides. This
repository is itself the filled form of that payload, which is what makes
divergence between the two halves visible at all.

Established by running the flow somewhere else, on 2026-09-17 and
2026-09-21: a different project can be bootstrapped from this payload, and a
feature can be driven through the plan loop with no access to this tree. The
trial built a git repository outside this repository from both halves, gave
an agent the owner answers a file cannot supply, and let it work. It ran
three times, once in each supported harness, with three separate sessions
each time — fill the artifacts, author a plan, execute that plan's first
milestone — and each run ended with a small command-line tool that records
labels and prints counts, a test suite behind a command the project chose
and published itself, and a plan whose later milestones were left open. The
three projects picked three different names for their stored data and three
different names for their verification command, and answered identically at
the terminal.

What the runs cost, as a sizing figure for anyone repeating it: eighteen to
thirty minutes of driven session time per harness for the three sessions
together, unattended.

Not established, and worth naming precisely because the rest now is. No
harness has been watched failing when the procedure set arrives damaged —
the one deliberately broken copy was bootstrapped anyway, because every
harness reached the procedures by reading the tree rather than through a
loader, so a wrong filename removed nothing any of them was using. Two of
the three runs judged a payload that has since been corrected and only their
first session was driven again, so exactly one harness has carried a feature
end to end through the payload as it stands. Every target was greenfield and
every target was the same tool, so nothing here says how the payload lands
on an existing codebase — `docs/DEBT.md` `D6` owns that — or on a project
shaped unlike a single-user command-line program.
