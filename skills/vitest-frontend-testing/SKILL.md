---
name: vitest-frontend-testing
description: Use when writing, reviewing, or fixing Vitest tests for rendered frontend behavior, user interactions, HTTP requests, asynchronous UI, or accessibility.
---

# Vitest Frontend Testing

Test rendered behavior as a user would. Use Testing Library, MSW for HTTP, and visible DOM assertions.

**Scope:** rendered frontend integration tests. Pure logic and Vue 3, Nuxt, or Pinia-specific setup belong in other skills.

## Workflow

1. Read the component's public inputs and user-visible outcomes. Identify every HTTP request it triggers.
2. Render with the project's Testing Library adapter. Exercise the UI through accessible elements, then assert the resulting DOM.
3. Register MSW handlers for every HTTP interaction. Cover success and meaningful failure states through the rendered UI.
4. Use `userEvent` for realistic interactions when it is installed; otherwise use Testing Library's `fireEvent`.
5. For asynchronous UI, wait for the expected result with `findBy*` or `waitFor`. Never use arbitrary delays.
6. Run the targeted test and any configured lint or type checks.

## Non-Negotiable Boundaries

| Situation | Required approach |
|---|---|
| Rendered UI | Testing Library adapter and `screen` |
| HTTP request | MSW handler at the network boundary |
| User interaction | `userEvent` when available; otherwise `fireEvent` |
| Async result | `findBy*` or `waitFor` on the visible outcome |
| Accessibility | Prefer role, label, and accessible-name queries; add an accessibility audit when the project supports one |

Do not use `vi.mock`, `vi.doMock`, `vi.mocked`, `vi.spyOn`, mock functions, module fakes, component stubs, shallow rendering, wrapper internals, emitted-event assertions, snapshots, or direct state mutation. These test implementation details instead of the application.

If a dependency cannot run in an integration test, do not replace it with a mock. Configure its real test-safe boundary, exercise it through MSW if it uses HTTP, or stop and ask for a testable integration path.

Do not impose test-file naming conventions. Follow the repository's existing test discovery and conventions.

## Query Order

1. `getByRole` with an accessible name
2. `getByLabelText`
3. `getByText` for user-visible copy
4. `getByTestId` only when no semantic query represents the element

## Common Failures

| Temptation | Use instead |
|---|---|
| Mock a fetch wrapper or API module | An MSW handler that returns the required HTTP response |
| Assert a callback or spy invocation | Assert the visible result of the user action |
| Sleep for a loading state | Wait for the resolved DOM state |
| Shallow-render a complex component | Render the real component and configure its real dependencies |
| Skip accessibility because it is not required | Use semantic queries at minimum; add the project's accessibility audit when practical |

## Red Flags

- "A module mock is faster."
- "A spy is only for verification."
- "This third-party dependency cannot run, so I will fake it."

All three replace the application boundary rather than testing it. Use an MSW handler for HTTP, configure the dependency's supported test-safe mode, or ask for a testable integration path.

See [the cookbook](references/VITEST_FRONTEND_COOKBOOK.md) for MSW lifecycle, async, and accessibility patterns.
