---
title: "Python style guide"
type: document
tags: ["#convention", "#python", "#style"]
status: published
version: 1
---

# Python style guide

## Formatter and linter
- **Ruff** for linting and formatting. One tool, zero overlap with Black/isort/Flake8.
- 88-column line length (Black-compatible default). Wider lines are allowed in test parameter tables and SQL strings.
- `pyproject.toml` `[tool.ruff]` is the source of truth — no per-file overrides except in generated code.

## Naming
- `snake_case` for functions, methods, variables, modules.
- `PascalCase` for classes and protocols.
- `UPPER_SNAKE_CASE` for module-level constants.
- Leading underscore (`_internal`) is private; do not import across package boundaries.

## Imports
- Absolute imports inside packages. Relative imports only inside a single subpackage.
- Sort: standard library, third-party, first-party, local — Ruff's `isort` plugin enforces this.
- No wildcard imports (`from x import *`) outside of `__init__.py` re-exports.

## Strings
- f-strings for interpolation. `.format()` only when the same template is reused or formatted lazily (logging).
- `repr()` in debug logs, `str()` in user-facing messages.

## Comments and docstrings
- Public functions, classes, and modules have docstrings. Google or NumPy style — pick one and stick with it.
- Comments explain *why*, never *what*. If the comment describes what the code does, rename the symbols instead.

## Tooling stack
- Ruff (lint + format)
- mypy (types — see [[types]])
- pytest (test runner — see [[testing]])
- uv or poetry for dependency management (see [[deps]])

## See also

[[error-handling]] · [[testing]] · [[deps]]
