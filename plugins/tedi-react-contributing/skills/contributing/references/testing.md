# Test & Verify TEDI React Component

Target: `$ARGUMENTS`

For test-writing patterns (controlled vs uncontrolled, mocking the breakpoint hook, query priority), see [best-practices.md → Testing Patterns](best-practices.md#testing-patterns). This file is about running tests and diagnosing failures.

## Workflow

### 1. Run Tests

If a component name or path was provided:
```
npm test -- --testPathPattern="$ARGUMENTS"
```

If no argument was provided, run the full test suite:
```
npm test
```

### 2. Analyze Failures

If tests fail:
- Read the failing test file and the component source.
- Identify the root cause — is it a test issue or a component bug?
- Fix the issue in the appropriate file.
- Re-run the failing test to confirm the fix.

### 3. Common React Testing Failure Modes

Recognize these patterns before treating them as component bugs:

- **`act()` warnings** — `Warning: An update to ComponentName inside a test was not wrapped in act(...)`. State updates triggered by async work (timers, promises, observers) need to settle inside an `act` boundary. Wrap the trigger in `await act(async () => { ... })`, or switch the assertion to `findBy*` / `waitFor` which already handle this.
- **Async query priority** — for state that appears asynchronously, prefer `findByRole` / `await waitFor(() => expect(...).toBe(...))` over `getBy*`. `getBy*` throws immediately if the element isn't there yet; `findBy*` waits up to the testing-library timeout. Don't sprinkle `setTimeout` to "wait" in tests.
- **Stale state across tests** — if a test passes in isolation but fails in the full suite, suspect shared module-level state (a singleton in `helpers/`, a leaked mock, a stale window listener). Run with `--detectLeaks` or reset the offender in `beforeEach`.
- **Breakpoint hook returning `undefined`** — if a breakpoint-aware component renders nothing in a test, the `useBreakpointProps` hook hasn't been mocked. See [best-practices.md → Mocking Breakpoint Hook](best-practices.md#mocking-breakpoint-hook).
- **`scrollIntoView is not a function`** — jsdom doesn't implement scroll APIs. If a component calls them, stub on the prototype in `setup-jest.ts` or guard the call with a feature check.
- **Form control fires twice** — usually means both the controlled `value` and the internal `innerValue` are driving the input. Re-check the controlled/uncontrolled merge logic.

### 4. Run Lint

```
npm run lint
```

### 5. Fix Lint Errors

If lint reports errors:
- Fix each reported issue in the source file.
- Re-run lint to confirm all errors are resolved.

### 6. Report

Summarize what was run and the outcome:
- Tests: pass/fail count
- Lint: clean or what was fixed
- Any issues that need manual attention
