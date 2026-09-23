# AGENTS Guidelines for This Repository

This repository contains a Next.js application located in the root of this repository. When
working on the project interactively with an agent (e.g. the Codex CLI) please follow
the guidelines below so that the development experience – in particular Hot Module
Replacement (HMR) – continues to work smoothly.

## 1. Use the Development Server, **not** `npm run build`

* **Always use `npm run dev` (or `pnpm dev`, `yarn dev`, etc.)** while iterating on the
  application.  This starts Next.js in development mode with hot-reload enabled.
* **Do _not_ run `npm run build` inside the agent session.**  Running the production
  build command switches the `.next` folder to production assets which disables hot
  reload and can leave the development server in an inconsistent state.  If a
  production build is required, do it outside of the interactive agent workflow.

## 2. Keep Dependencies in Sync

If you add or update dependencies remember to:

1. Update the appropriate lockfile (`package-lock.json`, `pnpm-lock.yaml`, `yarn.lock`).
2. Re-start the development server so that Next.js picks up the changes.

## 3. Coding Conventions

* Prefer TypeScript (`.tsx`/`.ts`) for new components and utilities.
* Co-locate component-specific styles in the same folder as the component when
  practical.

## 4. Feature State and Domain Transitions

When implementing application behavior, keep state changes behind the feature or domain
object that owns the invariant:

* Prefer intent-revealing transition functions/methods (for example `submitOrder`,
  `markComplete`, or `applySelection`) over directly assigning fields that already
  have a domain transition. Components, route handlers, and API handlers should
  coordinate input/output, persistence, and side effects instead of duplicating the
  transition by mutating domain state themselves.
* Preserve nested collection semantics explicitly. If a change replaces a child list
  rather than patching individual items, make that replacement behavior obvious in the
  owning state/domain object and cover it with a focused regression test.
* Keep fixes incremental: update the narrow transition surface needed for the task
  rather than broad refactors or new abstractions.

## 5. Useful Commands Recap

| Command            | Purpose                                            |
| ------------------ | -------------------------------------------------- |
| `npm run dev`      | Start the Next.js dev server with HMR.             |
| `npm run lint`     | Run ESLint checks.                                 |
| `npm run test`     | Execute the test suite (if present).               |
| `npm run build`    | **Production build – _do not run during agent sessions_** |

---

Following these practices ensures that the agent-assisted development workflow stays
fast and dependable.  When in doubt, restart the dev server rather than running the
production build.
