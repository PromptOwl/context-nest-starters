---
title: "Java style guide"
type: document
tags: ["#convention", "#java", "#style"]
status: published
version: 1
---

# Java style guide

## Toolchain

- **Spotless** with **google-java-format** for formatting. One config in `pom.xml` / `build.gradle.kts`. CI fails on diff.
- **Checkstyle** + **ErrorProne** for linting. Treat warnings as build errors.
- Target the current LTS (21+) — drop 8/11 support unless the deployment explicitly requires older.
- Records, pattern matching, sealed classes — use them.

## Naming

- `camelCase` for methods, variables.
- `PascalCase` for classes, interfaces, enums, records.
- `SCREAMING_SNAKE_CASE` for `static final` constants.
- Test classes end in `Test` (`UserServiceTest`).
- No Hungarian notation, no `I` prefix on interfaces.

## Package layout

- Reverse-domain package name: `com.<org>.<service>.<concern>`.
- One public type per file, named after the file.
- `internal` (lowercase) subpackages mark module-internal APIs (when using JPMS).

## Idioms

- **Prefer records over classes** for data containers. Immutable, equals/hashCode for free.
- **Sealed types for closed hierarchies.** Discriminated unions enabled.
- **Pattern matching for `instanceof`** (Java 16+). No casts after instanceof.
- **`var` for locals** when the right side makes the type obvious. Avoid `var` for complex generics.
- **Streams for collection transforms**, but loops are fine when more readable.
- **Optional for return values, never parameters.** `Optional<T>` returns vs nullable params — pick whichever the consumer needs.

## Null handling

- `null` is a code smell in new code. Use `Optional<T>` for absent values at API boundaries.
- Inside a class, use `@NonNull` / `@Nullable` annotations (JSpecify or JetBrains) consistently.
- Validate inputs at constructor / public method entry; assume non-null internally after.

## Comments

- Javadoc on every public class and method that crosses a module boundary.
- `@param`, `@return`, `@throws` documented for non-obvious behavior.
- Single-line comments explain *why*, not *what*.

## See also

[[error-handling]] · [[testing]] · [[deps]]
