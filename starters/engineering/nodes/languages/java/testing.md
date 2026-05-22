---
title: "Java testing conventions"
type: document
tags: ["#convention", "#java", "#testing"]
status: published
version: 1
---

# Java testing conventions

## Stack

- **JUnit 5** (Jupiter). No JUnit 4 in new code.
- **AssertJ** for assertions — fluent, chainable, better diagnostic output than Hamcrest.
- **Mockito** for mocking. Don't reach for it first; prefer real objects when feasible.
- **Testcontainers** for integration against real databases / brokers.
- **WireMock** for HTTP boundary mocking.
- **ArchUnit** for architectural rules tested as code.

## Structure

- `src/test/java/<package>/` mirrors `src/main/java/<package>/`.
- `<Class>Test.java` for unit tests; `<Class>IT.java` for integration tests (Maven Surefire vs Failsafe split).
- Helpers in `src/test/java/<package>/support/`.

## Naming and shape

```java
@Test
void charge_succeeds_when_card_valid() {
    // arrange / act / assert
}
```

Snake_case test names read as sentences. Optional `@DisplayName` for human-readable reports.

## Parameterized tests

```java
@ParameterizedTest
@CsvSource({
    "a, 1",
    "b, 2",
})
void lookup_returns_value(String input, int expected) {
    assertThat(lookup(input)).isEqualTo(expected);
}
```

## What to test

- Public API across success and documented failure paths.
- Concurrency contracts.
- Boundary conditions.

## What not to test

- Generated code (records, Lombok output).
- Trivial getters/setters that have no logic.
- Third-party library behavior.

## Mocking discipline

- Mock at the dependency seam (interface or external client). Don't mock your own concrete classes.
- Prefer test doubles you can read (fake repository implementations) over Mockito argument captors.
- ArgumentCaptor sparingly; AssertJ has cleaner alternatives via `assertThat(savedEntity).hasFieldOrPropertyWithValue(...)`.

## Coverage

80% on new code; legacy ratchet for existing packages. See [[../../qa/coverage-targets]].

## See also

[[error-handling]] · [[../../qa/test-strategy]] · [[../../testing/integration-conventions]]
