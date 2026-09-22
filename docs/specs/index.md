# Behavior specs

## The reflection rule

A plan does not complete until its behavioral outcome is reflected here.
Before a plan moves to `plans/completed/`, whatever a user or caller can now
observe that they could not before is written into a spec file in this
directory — as a new file, or as an edit to the file that already owns that
behavior — and listed in the index below.

A plan whose outcome is purely internal (a refactor, a dependency bump)
reflects nothing and says so in its own Outcomes section. Silence is only
acceptable when it has been stated.

Spec files are overwritten freely: they describe the present, so an edit
that contradicts last month's text is a correct edit, not a lost one. The
history lives in version control and in `plans/completed/`.

## Index

One row per spec file, with a narrow statement of what that file covers.

| Spec | Covers |
| --- | --- |
| `bootstrap-flow.md` | What this repository offers a target project, what a project has once the payload and the procedures are installed, and which of those claims have been observed — including what three live trials in three harnesses settled and what they left open |
| `mechanical-checks.md` | The three commands a reader can run and how they differ in subject, what each of the six checks decides, and which of this repository's claims are still read by a person |
