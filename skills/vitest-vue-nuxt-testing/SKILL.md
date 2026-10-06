---
name: vitest-vue-nuxt-testing
description: Use when writing, reviewing, or fixing Vitest tests for Vue 3 components, Nuxt runtime behavior, auto-imports, async setup, or Pinia stores.
---

# Vitest Vue and Nuxt Testing

**REQUIRED BASELINE:** Apply `vitest-frontend-testing` first.

## Render the Real Application

Use `renderSuspended` from `@nuxt/test-utils/runtime` for Nuxt components and composables. Use `screen`, `within`, and user interactions from `@testing-library/vue`.

Do not use `mountSuspended`, `mount`, `shallowMount`, wrapper access, `wrapper.vm`, `.emitted()`, `setData`, component stubs, or direct `@vue/test-utils` APIs. Supply public props, interact through the DOM, and assert visible outcomes.

```ts
import { expect, it } from 'vitest'
import { renderSuspended } from '@nuxt/test-utils/runtime'
import { screen } from '@testing-library/vue'
import CartPanel from './CartPanel.vue'

it('shows the empty-cart message', async () => {
  await renderSuspended(CartPanel, { props: { cartId: 'empty' } })

  expect(screen.getByRole('status')).toHaveTextContent(/cart is empty/i)
})
```

## Nuxt Runtime

Let Nuxt provide its real auto-imports and plugins. Do not mock `useRoute`, `useRouter`, `useAsyncData`, `useFetch`, `useRuntimeConfig`, or other internal composables. Set public route or runtime inputs through the project's supported Nuxt test setup, then test the resulting UI.

Use MSW for `$fetch`, `useFetch`, and every other HTTP request. Do not replace them with `vi.stubGlobal`, module mocks, or fake composable results.

## Pinia

Use the real Pinia instance supplied by the Nuxt application. Do not use `createTestingPinia`, action stubs, state injection, mocked actions, or spies.

For a page or component, trigger store behavior through the DOM and assert rendered state. For a Nuxt-aware store, call its public action in the real Nuxt runtime and assert public state or observable HTTP-backed behavior. Do not mutate refs or state to arrange a result.

## i18n and Plugins

Run real app plugins. Do not stub i18n, child components, or plugin-provided composables. Assert the user-visible localized text configured for the test environment. If a plugin has no supported test-safe configuration, ask for one rather than replacing it.

## Review

| Check | Required result |
|---|---|
| Renderer | `renderSuspended` plus Testing Library DOM APIs |
| Nuxt composables | Real runtime behavior, no auto-import mocks |
| HTTP | MSW handlers, including meaningful error paths |
| Pinia | Real store and public actions only |
| Assertions | Visible DOM or public store state, never wrappers or emitted events |
| Accessibility | Semantic Testing Library queries; add the project audit when available |

## Red Flags

| Rationalization | Correct response |
|---|---|
| "`useRoute` is hard to set up." | Configure the real route through the Nuxt test environment. |
| "`createTestingPinia` is the quickest option." | Use the real store and drive it through its public interface. |
| "The child is unrelated." | Render it with its real plugins, or get a supported test-safe configuration. |
| "I only need to check the emitted event." | Assert the user-visible state after the interaction. |

Do not impose test-file naming rules. Follow the repository's existing test discovery and conventions.
