# Turborepo bug report

Can't pass arguments to the underlying command when using `--filter`.

## Scenario

In this project, run:

```
npm run test -- --filter=ui -- --passWithNoTests
```

<details>
<summary>Terminal log</summary>

```
> turborepo-args-bug@0.0.0 test
> turbo run test "--filter=ui" "--" "--passWithNoTests"

• Packages in scope: ui
• Running test in 1 packages
ui:test: cache miss, executing 0712952f7221f408
ui:test:
ui:test: > ui@0.0.0 test
ui:test: > jest
ui:test:
ui:test: No tests found, exiting with code 1
ui:test: Run with `--passWithNoTests` to exit with code 0
ui:test: In /Users/ivolimasilva/Playground/turborepo-args-bug/packages/ui
ui:test:   4 files checked.
ui:test:   testMatch: **/__tests__/**/*.[jt]s?(x), **/?(*.)+(spec|test).[tj]s?(x) - 0 matches
ui:test:   testPathIgnorePatterns: /node_modules/ - 4 matches
ui:test:   testRegex:  - 0 matches
ui:test: Pattern:  - 0 matches
ui:test: npm ERR! Lifecycle script `test` failed with error:
ui:test: npm ERR! Error: command failed
ui:test: npm ERR!   in workspace: ui@0.0.0
ui:test: npm ERR!   at location: /Users/ivolimasilva/Playground/turborepo-args-bug/packages/ui
ui:test: ERROR: command finished with error: command (packages/ui) npm run test --passWithNoTests exited (1)
command (packages/ui) npm run test --passWithNoTests exited (1)

 Tasks:    0 successful, 1 total
Cached:    0 cached, 1 total
  Time:    1.878s
```
</details>

### Expected

To run `npm run test -- --passWithNoTests` only for `ui` package.

### Reality

It runs `npm run test` only for `ui` package without passing the option `--passWithNoTests`.

---

## Notes

It is however possible to pass arguments to the underlying command if the argument has no `-` or `--`.

E.g. `npm run test -- --filter=ui -- Button` will run tests for the `ui` package filtering only files with the expression `Button`.
