# Capabilities

## Register

One row per card file in this directory, with `Status` one of `specced`,
`built`, or `enforced` and `Enforced at` naming the concrete command, hook, or
job while reading `—` for anything still `specced`. `CARD_FORMAT.md` in this
directory defines what each status requires, and this table is the only place
a status is recorded.

Cards arrive here two ways: this project writes its own, and it instantiates
the ones the payload ships in `template/docs/capabilities/` that state an
invariant holding here too.

| Card | Status | Enforced at |
| --- | --- | --- |
| `template-live-drift` | enforced | `./tools/verify`, `tools/hooks/pre-commit` |
| `blueprint-eval` | specced | — |
| `loop-runner` | enforced | `./tools/verify`, `tools/hooks/pre-commit` |
| `fast-verify` | enforced | `./tools/verify`, `tools/hooks/pre-commit` |
| `doc-integrity` | enforced | `./tools/verify`, `tools/hooks/pre-commit` |
| `prose-duplication` | enforced | `./tools/verify`, `tools/hooks/pre-commit` |
| `evidence-check` | enforced | `./tools/verify`, `tools/hooks/pre-commit` |
| `boundary-lint` | enforced | `./tools/verify`, `tools/hooks/pre-commit` |
