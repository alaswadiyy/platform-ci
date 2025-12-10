# platform-ci

Reusable, repository-scoped GitHub Actions workflows and CI building blocks for Go projects.

## Overview

`platform-ci` provides reusable GitHub Actions workflows and example configurations designed to make it easy
to add consistent CI pipelines across Go repositories. Workflows are authored to be composable via
`workflow_call` so other repositories (or this repo's workflows) can call them with inputs.

## Features

- Reusable workflow for running Go tests across OS and Go version matrices
- Simple inputs for `working-directory` and `go-module-path`
- Examples that work on Ubuntu, macOS and Windows runners

## Quick Start

1. Copy or reference the reusable workflow from this repository into your repository.
2. Call it from your repository's workflows using `uses` with `workflow_call` inputs.

### Call the workflow from another workflow (same repo)

```yaml
name: Run shared CI

on:
  push:
    branches: [ main ]

jobs:
  call-ci:
    uses: ./.github/workflows/go-matrix-test.yml
    with:
      working-directory: "./"
      go-module-path: "./"
    secrets: inherit
```

### Call the workflow from a different repo

```yaml
jobs:
  call-ci:
    uses: alaswadiyy/platform-ci/.github/workflows/go-matrix-test.yml@main
    with:
      working-directory: "./"
      go-module-path: "./"
    secrets: inherit
```

Note: When calling a reusable workflow from a different repository, pin a stable tag or commit instead of `main` for reproducible CI.

## Provided Workflow: `go-matrix-test.yml`

This workflow defines a `workflow_call` that accepts inputs:

- `working-directory` (string, optional) — working directory where `go test` is run (default `.`)
- `go-module-path` (string, optional) — module path to use when setting `GOMOD` or changing directories (default `./`)

It runs tests across a matrix of Go versions and operating systems using `actions/setup-go` and `go test ./...`.

## Development & Testing Locally

- There is no direct local runner for GitHub Actions included here. To iterate on workflows quickly:
  - Use small, incremental changes and push to a feature branch.
  - Use `act` (third-party) to run GitHub Actions locally for quick validation (note: matrix OS behavior is limited).

---