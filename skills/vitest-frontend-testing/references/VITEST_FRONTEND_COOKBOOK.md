# Vitest Frontend Testing Cookbook

## MSW Lifecycle

Use one server lifecycle for integration tests. The exact setup-file location is repository-specific.

```ts
import { afterAll, afterEach, beforeAll } from 'vitest'
import { setupServer } from 'msw/node'

export const server = setupServer()

beforeAll(() => server.listen({ onUnhandledRequest: 'error' }))
afterEach(() => server.resetHandlers())
afterAll(() => server.close())
```

`onUnhandledRequest: 'error'` catches requests that would otherwise escape the test or silently use an unplanned response.

## HTTP States

Describe each externally observable state with an MSW response and DOM assertion.

```ts
server.use(
  http.get('/api/profile', () => HttpResponse.json({ message: 'Unavailable' }, { status: 503 })),
)

render(ProfilePanel)

expect(await screen.findByRole('alert')).toHaveTextContent(/unavailable/i)
```

Avoid asserting a request function's call count or arguments. The rendered result is the contract.

## Async UI

```ts
await user.click(screen.getByRole('button', { name: /refresh/i }))

expect(await screen.findByRole('heading', { name: /latest results/i })).toBeVisible()
```

Use `waitFor` only when no `findBy*` query expresses the expected result. Do not add `setTimeout`, timer sleeps, or polling loops.

## Accessibility

Accessible queries are required because they represent the interface available to users. An automated accessibility audit is optional but strongly recommended when the project includes an audit tool.

Audit representative visible DOM with the tool's default configuration. Do not hide nodes, disable rules, or alter production markup solely to make an audit pass. Report unrelated existing violations rather than adding test-only workarounds.

## Review Checklist

- The test renders through Testing Library and asserts visible behavior.
- Every HTTP request has an MSW handler.
- No mock, spy, stub, shallow render, snapshot, or implementation-detail assertion exists.
- User interactions use accessible elements.
- Async assertions wait for the actual UI outcome.
- An accessibility audit was considered and added when supported.
