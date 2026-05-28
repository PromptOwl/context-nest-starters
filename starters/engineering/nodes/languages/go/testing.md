---
title: "Go testing conventions"
type: document
tags: ["#convention", "#go", "#testing"]
status: published
version: 1
---

# Go testing conventions

## Stack

- **Standard library `testing`** is the runner. No third-party runner.
- **testify/require** is acceptable for assertions where standard library is verbose; keep it inside test helpers.
- **gomock** or **mockery** for interface mocking — only when the seam is genuine.
- **dockertest** or **testcontainers-go** for integration tests requiring real dependencies.

## Structure

- `xxx_test.go` co-located with `xxx.go` in the same package — black-box tests are in `xxx_test.go` with `package xxx_test`.
- One `Test<Function>` per public function. Sub-tests for variations.

```go
func TestParse(t *testing.T) {
    t.Run("succeeds on valid input", func(t *testing.T) { ... })
    t.Run("fails on empty input", func(t *testing.T) { ... })
}
```

## Table-driven tests

```go
tests := []struct {
    name   string
    input  string
    want   int
    wantErr bool
}{
    {"empty", "", 0, true},
    {"single", "a", 1, false},
}
for _, tt := range tests {
    t.Run(tt.name, func(t *testing.T) {
        got, err := Parse(tt.input)
        if (err != nil) != tt.wantErr { t.Fatalf("err = %v, wantErr = %v", err, tt.wantErr) }
        if got != tt.want { t.Errorf("got %d, want %d", got, tt.want) }
    })
}
```

Every case is a separate sub-test in the report.

## What to test

- Public API behavior across success and documented failure paths.
- Concurrency contracts (race tests with `go test -race`).
- Boundary conditions: empty, nil, max-size, unicode edge cases.

## What not to test

- Mocked code paths where the test only verifies the mock was called.
- Side-effect-free trivialities.
- Code paths that exist solely for tests.

## Race detector

- `go test -race ./...` runs in CI. Race-detected code blocks the PR.
- Locally, run with `-race` whenever touching concurrent code.

## Coverage

80% on new packages; existing legacy packages have their own ratchet. See [[../../qa/coverage-targets]].

## See also

[[error-handling]] · [[concurrency]] · [[../../qa/test-strategy]]
