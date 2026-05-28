---
title: "TypeScript style guide"
type: document
tags: ["#convention", "#typescript", "#style"]
status: published
version: 1
---

# TypeScript style guide

## Toolchain

- **Prettier** for formatting. No flags-bikeshedding; accept defaults except `"singleQuote": true`.
- **ESLint** with `@typescript-eslint` for linting. Use the team's flat config (`eslint.config.js`).
- `"strict": true` in `tsconfig.json`. Non-negotiable.
- `"noUncheckedIndexedAccess": true`. Catches `arr[0]` being `T | undefined`.

## Naming

- `camelCase` for variables, functions, methods.
- `PascalCase` for types, interfaces, classes, enums, React components.
- `UPPER_SNAKE_CASE` for module constants.
- `_unused` prefix to satisfy `noUnusedParameters` for required-but-unused args.

## Imports

- Path aliases via `tsconfig.json` `"paths"` (`@/components/...`). Relative imports only within a single folder.
- Import sort order enforced by ESLint plugin: builtins → external → internal-alias → relative.
- No default exports for components or utilities — named exports only. Default exports are tolerated only for framework-required entrypoints (Next.js page files).

## Types

- Prefer `type` over `interface` except when you need declaration merging.
- `unknown` over `any`. Any time you write `any`, leave a comment explaining why.
- Discriminated unions for state machines: `{ kind: "loading" } | { kind: "ok"; data: T } | { kind: "error"; err: Error }`.
- Branded primitives for IDs to prevent mixing: `type UserId = string & { __brand: "UserId" }`.

## React-specific (where applicable)

- Function components only. No class components in new code.
- Hooks rules enforced by ESLint plugin. Custom hooks named `useX`.
- Co-locate component + its tests + its CSS modules.

## See also

[[error-handling]] · [[testing]] · [[deps]]
