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
| `bootstrap-flow.md` | What this repository offers a target project, what the bootstrap interview asks and records — the optional provenance question included — what a project holds once payload and procedures are installed, and the observed record behind those claims: the 2026-09-22 per-harness bootstrap re-runs, the reachability outcome and report shape each harness produced, which older records they superseded, and what stays unobserved |
| `check-protocol.md` | The contract any check conforms to — invocation, exit meanings and output shapes, the allowlist file format, the aggregator's ordering and streaming, and how the published time budget is set |
| `mechanical-checks.md` | The three commands a reader can run and how they differ in subject, what each of the seven checks decides, and which of this repository's claims are still read by a person |
