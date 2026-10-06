# Skills

A small collection of reusable skills for quality minded people working with AI.

Each skill is written for a real person to read too, so don't hesitate to do it! They are a collection of my personal preferences and experiences, to be used as a reference when working with AI. Keep in mind, LLMs being non-deterministic, these skills do not replace linters and other similar tools.

## Install

```sh
pnx skills add nicolassutter/skills
# bunx skills add nicolassutter/skills
# npx skills add nicolassutter/skills
```

## Available Skills

### Vitest Frontend Testing

Use `vitest-frontend-testing` for rendered frontend integration tests. It uses Testing Library for user-facing behavior, MSW for HTTP, and encourages accessible, non-flaky tests without mocks or spies.

### Vitest Vue and Nuxt Testing

Use `vitest-vue-nuxt-testing` when the test runs in Vue 3, Nuxt, or Pinia. It builds on the frontend-testing skill with real Nuxt runtime and real Pinia stores.

## Adding Skills

Keep skills focused and portable. Put framework-specific guidance in a companion skill when it adds real value, avoid project-specific rules.
