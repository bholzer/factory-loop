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

No spec files exist yet. What this project offers an outside reader is the
payload in `template/` and the skills that operate it, and neither is
complete enough to describe in the present tense without describing a
half-built thing. The first spec lands when the bootstrap flow can be
followed end to end; it will cover what an agent running that flow in a
target project gets, and this section becomes a table with one row per spec
file and a narrow statement of what each one covers.
