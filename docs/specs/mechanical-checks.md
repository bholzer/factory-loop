# What this repository checks

## What a reader can run

Two commands, and only the first of them runs on every edit. `./tools/verify`
decides everything a machine decides about this repository itself. `AGENTS.md`
publishes it under Commands together with the time it is allowed to take, and
`docs/capabilities/fast-verify.md` is the card that says what it owes whoever
runs it: the executables under `tools/checks/` in an order written down rather
than globbed, each one's own output passed through untouched, and a nonzero exit
as soon as any of them reports a violation or declines to decide. Six run today.
A clean pass ends in one line counting the checks and the seconds spent, and
those seconds have stayed an order of magnitude inside the published figure
since the first check landed.

`./tools/blueprint-eval` is the other one, and its subject is not this
repository. It builds a throwaway git repository outside this tree from both
halves and then reads that copy back: whether the procedures arrived in the
shape every supported harness expects, whether any authoring scaffolding
outlived the bootstrap, and whether the copy's own paths lead anywhere inside
the copy. `docs/capabilities/blueprint-eval.md` is its card, and those three
questions are the part of it a machine can settle — the rest is a person
starting an agent in that copy and reading what it did. It is deliberately not
one of the executables the cheap command runs. What it examines exists only
while a trial is under way — a project's own answers written over the
skeletons, in a directory that never appears in a commit here — so wiring it
into the budget would spend that budget on a subject that is usually missing.

Beside those two is the one-time install line in that same section, which
points git at the hook directory this repository keeps in version control.
After it, a commit the command rejects is refused before it is written. That
refusal is the whole gate: nothing runs after a push, so what a committer can
still do to get past it is `docs/DEBT.md` `D8`.

## What each check decides

Each paragraph below names the card that owns the property. The card carries the
failing case and the exact text the failure prints; the executable beside it
under `tools/checks/` is where the decision is made, and neither one is a second
copy of the other.

`tools/checks/scaffolding-markers` is the only check with no card. It looks for
the authoring markers a filled artifact should no longer be carrying, over the
eight live files that were filled from skeletons, and the marker syntax itself
belongs to the payload's own agent guide rather than to a capability.
`docs/DEBT.md` `D2` holds what it cannot tell apart.

`docs/capabilities/template-live-drift.md` owns the relationship between the two
halves: whether each payload file still has its live counterpart, whether the
one pair that must agree to the byte still does, and whether the headings a
skeleton declares still appear in its live form in the order the skeleton put
them. Card files and directory placeholders are outside it for reasons the card
gives, and it reads the payload against the live tree and never the reverse.

`docs/capabilities/doc-integrity.md` owns whether the paths the live artifacts
write down can be followed. A reference is a backticked span or a link target
that carries a directory separator and does not open with one — an absolute
path names a location on the machine, not one here; quoted material is not a
citation, plan files are outside the checked set entirely, and a mention that
must not resolve lives in `tools/allow/doc-integrity.txt` with the reason it is
there.

`docs/capabilities/prose-duplication.md` owns whether the same sentence has been
written down twice. It compares runs of eight words across both halves after
stripping quoted material, and the classes it treats as legitimate — copies a
declared correspondence requires, text a shared format imposes, a restatement
that names its owner — are enumerated on the card.

`docs/capabilities/evidence-check.md` owns whether work in flight is recorded:
the four living sections a plan is required to carry are present with something
under each, and an entry ticked as finished says when it was observed. An empty
`plans/active/` is a pass, which the card states deliberately.

`docs/capabilities/boundary-lint.md` owns the direction of every reference in
the tree. The rules and their identifiers live in `ARCHITECTURE.md`, inside the
layer map that states why each one exists; the check binds to those identifiers
and stops with an undecided exit when they and the implementation disagree in
either direction, which is what keeps a reworded paragraph from retiring a rule
in silence.

## What each check leaves to a human

Three of the properties this repository claims have no mechanism at all, and
each one is a row in `docs/DEBT.md` rather than a gap anybody has to
rediscover. `D9` holds the two questions the correspondence rules raise that
no comparison of text can answer. `D10` holds the clause of the layer map that
would need an enumeration of every harness's tool names. `D11` holds the two
format documents, whose copies are compared as if a project filled them in.

Three more are deliberate boundaries of checks that do run. A shared window
locates a fact in two files without saying which file owns it, so the routing is
the reader's. A reference the reader chose to leave dangling, and a window two
files genuinely must both spell out, are judgement calls recorded as allowlist
entries with a reason, and an entry that has stopped matching anything is
counted in the passing output rather than deleted quietly. And whether a
plan's recorded evidence is true — whether a command whose output is quoted was
actually run — is the working rule in `AGENTS.md` that a person enforces by
reading the diff; nothing here can decide it, and a check claiming to would be
the reassuring nothing `docs/capabilities/CARD_FORMAT.md` warns against.

One more is structural. With no plan in flight the evidence check passes over an
empty directory, so its green run says nothing at all until the next plan opens;
the scope is legitimately empty, which is the one place in this set where a pass
should not be read as a finding of correctness.
