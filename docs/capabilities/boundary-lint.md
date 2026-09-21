# boundary-lint

## Invariant

No reference in this repository points in a direction the layer map in
`ARCHITECTURE.md` forbids. That file owns which directions are allowed and
why; this card owns only that every reference is checked mechanically on every
change.

Nothing here compiles, so an edge is a path reference from one component's
files into another's, derived from file content and never from a list anyone
maintains by hand — such a list drifts the moment someone adds a reference. A
reference is a backticked span or a markdown link target containing a slash
and none of space, `*`, angle bracket, brace, or colon. The colon is what
keeps a file-and-line locator and a compound allowlist key out of the set:
both contain a slash and neither is a path.

Three of the layer map's four rules are decidable, and the check binds to
their ids rather than to the prose that states them, so that rewording a
paragraph cannot retire a rule silently:

- `no-outward-payload-reference` — every path reference in a file under
  `template/` resolves inside the payload, and no file there mentions this
  repository, the blueprint, or the procedure directory. A reference resolves
  when the path exists under `template/` or when its parent directory does;
  the parent-directory allowance is what makes an illustrative filename
  legitimate, since a card's own remediation transcript names a file that must
  not exist while the directory holding it is shipped.
- `no-unprovided-skill-target` — every path reference in a file under
  `skills/` names something the payload ships or a procedure beside it, and no
  skill body names the payload directory. There is deliberately no
  parent-directory allowance here, because the rule is precisely that a
  project's own card, spec, decision record or plan inside a shipped directory
  is not namable.
- `no-plan-file-dependency` — no file outside `plans/` references a file under
  `plans/active/` or `plans/completed/` that exists. The convention document
  `plans/PLANS.md` is not a plan and is named freely.

The check must fail rather than pass when it cannot find rules to apply. A
missing map, an absent or empty rule-id block, or an id set that differs from
the implemented one in either direction is `cannot run` — exit 2 — because an
id with no implementation is a rule nobody enforces and an implementation with
no id is a rule nobody wrote down. A green run over zero rules is worse than
no check, since its output is read as evidence.

Unlike `docs/capabilities/doc-integrity.md`, this check reads fenced and
indented blocks like any other line. A dangling path inside a payload
transcript is still a path the reader of the copy follows, which is why the
parent-directory allowance carries the illustrative cases instead. The
consequence for an author: a payload file that must name a foreign path — a
per-stack hint naming a source tree this project does not have — writes it
unbackticked, or it is a violation of the rule rather than an example of it.

One clause of the layer map is not decided here. A skill body may not name a
harness-specific tool, and deciding that needs a list of every tool name in
every harness that nobody can write; review owns it, and `docs/DEBT.md` `D10`
records it so the gap reads as routed rather than forgotten.

## Enforcement point

`./tools/verify` from the repository root, and the versioned hook
`tools/hooks/pre-commit`, which runs that same command and refuses the commit
when it exits nonzero. There is no continuous integration to name: nothing runs
on the remote this repository pushes to, and the gate's skippability is
`docs/DEBT.md` `D8`.

## Acceptance

Passing case: on a clean checkout the check exits zero and prints one summary
line stating how many references it examined, across how many files, how many
rules it applied from the layer map, and that the banned-mention scan found
nothing across the payload files and the procedures. The reference count and
the rule count must both be greater than zero; the rule count is the number of
ids in the map's block, so a run reporting fewer than three has lost a rule.

Failing case: add a line to any file under `template/` citing the backticked
path of this repository's verification command — a path that resolves here and
nowhere inside the payload, which is exactly the outward reference the first
rule bans — then run `./tools/verify` and observe a nonzero exit naming the
offending file, its line number, the target, and the rule id. Revert the line
and observe the pass return. Then, separately, rename `ARCHITECTURE.md` and
observe exit 2 saying the layer map could not be read rather than a pass over
zero rules. Restore the name. Do both before marking the card `built`.

## Remediation message

    boundary-lint: template/docs/DEBT.md:69 names `tools/verify`, which does
    not resolve inside the payload.
      ARCHITECTURE.md no-outward-payload-reference: nothing under template/
      may name a path outside template/, because a target project receives
      that directory with no ancestor context and the reference dangles on
      arrival.
      Point it at an artifact the payload ships, or write it unbackticked if
      it is an illustration rather than a path. If the rule itself is wrong,
      change ARCHITECTURE.md in this same commit and say why in the plan's
      Decision Log.

Every block names the offender by file and line, the target, the rule id it
broke, and the two legitimate fixes plus the one legitimate way to change the
rule. "Boundary violation under template/" fails this bar: the reader still
has to find the reference.

Four other shapes exist, differing only in the offender and the rule cited: a
procedure naming an artifact the payload does not ship; a payload file
mentioning this repository, the blueprint, or the procedure directory; a skill
body naming the payload directory; and an artifact outside `plans/`
referencing a plan file that exists. A line already reported for its path
reference is not reported again by the mention scan, because one word fixed
once is one violation.

## Per-stack hints

No toolchain applies: `ARCHITECTURE.md` states the constraint that every
artifact here is markdown operated on with shell and git. One `awk` pass reads
the rule ids out of the layer map section, a second extracts every backticked
span and link target, and `test -e` decides each reference against the payload
tree. The import-graph tools the generic card suggests — `import-linter`,
`dependency-cruiser`, `go-arch-lint` — have nothing to read here, because
there are no imports: the dependency edges of a document system are its
cross-references.
