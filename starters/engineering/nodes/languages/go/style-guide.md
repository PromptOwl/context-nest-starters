---
title: "Go style guide"
type: document
tags: ["#convention", "#go", "#style"]
status: published
version: 1
---

# Go style guide

## Toolchain

- `gofmt` (or `gofumpt` if the team wants stricter rules) is the formatter. No discussion.
- `golangci-lint` is the linter. Enabled linters at minimum: `govet`, `staticcheck`, `errcheck`, `gosimple`, `revive`, `goconst`.
- Go version: track current stable, allow one minor behind for stability. Pin via `go.mod` `go 1.x`.

## Naming

- `MixedCaps` exported, `mixedCaps` unexported. No underscores in Go names.
- Receiver names are 1–2 letters consistent across all methods of the type: `func (s *Server)`, not `func (this *Server)`.
- Acronyms: `URL`, not `Url`. `HTTPSServer`, not `HttpsServer`.
- Test functions: `TestThing_Scenario(t *testing.T)`.

## Package layout

- Flat package, files grouped by concern (`handlers.go`, `models.go`, `errors.go`). Not by type.
- `cmd/<binary>/main.go` is the entrypoint pattern.
- `internal/` for code that must not be imported by other modules.
- One package per directory; package name matches directory.

## Idioms

- **Accept interfaces, return structs.**
- Zero values are useful. Designing a type so its zero value works avoids "newX" constructors.
- Slices and maps are reference-like; passing them lets the callee mutate. Document when that matters.
- Channels: small, single-purpose. Don't make a Channel-Of-Anything as a substitute for proper types.
- `context.Context` is the first parameter when one is used. Never store a Context in a struct field.

## Comments

- Public symbols have a doc comment starting with the symbol name: `// User represents an authenticated principal.`
- Internal comments explain *why*, not *what*. Idiomatic Go reads itself.

## See also

[[error-handling]] · [[testing]] · [[concurrency]] · [[deps]]
