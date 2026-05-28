---
title: "Python testing conventions"
type: document
tags: ["#convention", "#python", "#testing"]
status: published
version: 1
---

# Python testing conventions

## Stack

- **pytest** as the runner. Not unittest, not nose.
- **pytest-cov** for coverage (target 80% on new code; see [[../../qa/coverage-targets]]).
- **hypothesis** for property-based testing where invariants are clearer than examples.
- **freezegun** for time-dependent tests.
- **responses** or **respx** for HTTP mocking. Avoid mocking at the `requests` level; mock the protocol.

## Structure

- Tests live in `tests/` mirroring the package layout: `src/foo/bar.py` → `tests/foo/test_bar.py`.
- One `test_<function-name>` per behavior, not per function. Each test asserts one thing.
- Use `pytest.fixture` for setup. Module-scoped for expensive resources (DB connection), function-scoped for everything else.

## Naming and shape

```python
def test_<unit>_<scenario>_<expected>():
    # arrange
    # act
    # assert
```

Example: `test_charge_succeeds_when_card_valid()`. Read the test name; you know what it asserts without opening the body.

## Parametrize, don't loop

```python
@pytest.mark.parametrize("input,expected", [
    ("a", 1),
    ("b", 2),
])
def test_lookup_returns_value(input, expected):
    assert lookup(input) == expected
```

Each row is a separate test in pytest's report. Loops hide failures.

## Mocking discipline

- Mock at the boundary of *the unit under test*. Don't mock what you don't own.
- Prefer `monkeypatch` for env vars and module-level functions. Use `unittest.mock` for object methods.
- Never mock the database in integration tests — see [[../../testing/integration-conventions]].

## What not to test

- Code that's already covered by a library's tests (don't re-test `requests`).
- Trivial getters/setters.
- Configuration values.

## See also

[[error-handling]] · [[../../qa/test-strategy]] · [[../../qa/coverage-targets]]
