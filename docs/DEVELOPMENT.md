# Development

## Workspace model

The repository begins as a Python package managed by `uv` with a root `Makefile` providing stable project commands. It evolves into a polyglot monorepo when the FastAPI and React milestones begin.

```text
Foundation:  Python, Parquet, DuckDB, research, reporting
API phase:   FastAPI and generated OpenAPI contract
Web phase:   React, generated API client, presentation components
Async phase: worker, durable job state, queue, and supporting services
Tooling:     Make, Ruff, mypy, pytest, then ESLint, TypeScript, and Vitest
```

Locked dependency files are committed. Application commands are invoked through workspace tooling rather than global package installations.

## Prerequisites

The initial foundation requires:

- Git;
- Python version defined by the repository;
- `uv`;
- Make.

Node.js, `pnpm`, Docker, and Docker Compose become prerequisites only when their corresponding milestones are implemented. Exact versions are pinned with each workspace addition.

## Foundation bootstrap

The first code milestone generates one Python package and then reduces the generated output to the accepted command surface:

```bash
uv init --package
```

The package initially contains dataset contracts, public-source acquisition, validation, research, and reporting modules under one import namespace. Modules move into separate workspace packages only after working boundaries justify the additional packaging cost.

## Later application generation

FastAPI, worker, and React shells are generated in their respective milestones:

```bash
uv init --app apps/api
uv init --app apps/worker

corepack enable
pnpm create vite apps/web --template react-ts
```

The exact command sequence is recorded in the pull request that introduces each application because generators and their output change between tool versions. Generated output must be reviewed, reduced to the accepted architecture, and committed with the tool versions used.

## Stable project commands

The foundation exposes only commands backed by implemented behavior:

```bash
make bootstrap
make format
make lint
make typecheck
make test
make check
make data
make validate
make report
```

Later milestones add `make dev` and `make api-client`. `make check` is the local equivalent of the required pull-request CI checks.

## Package managers

### Python and `uv`

`uv` owns Python dependency resolution, virtual environments, workspace membership, and the committed lockfile.

Expected operations:

```bash
uv sync
uv add --package <package> <dependency>
uv add --package <package> --dev <dependency>
uv run pytest
```

Dependencies are added to the package that imports them. A dependency is not added to the repository root only to make a local import happen to work.

### TypeScript and `pnpm`

After the web milestone begins, `pnpm` owns the frontend workspace and the committed lockfile.

Expected operations:

```bash
pnpm install
pnpm --filter web dev
pnpm --filter web test
pnpm --filter web build
```

Package scripts remain callable through the root `Makefile` so CI and local development use the same entry points.

## API contract generation

FastAPI is the source of truth for the HTTP OpenAPI contract. The TypeScript API client is generated and committed or deterministically rebuilt according to the selected client generator policy.

```text
Python request and response models
        -> FastAPI OpenAPI document
        -> TypeScript client and types
        -> React query hooks
```

Frontend code must not manually duplicate API enums and result schemas when generation is possible. CI verifies that generated output is current.

## Git workflow

### Protected default branch

The default branch must be protected by an active GitHub repository ruleset targeting `main` before the foundation pull request is merged.

Required policy:

- changes reach `main` only through pull requests;
- at least one approval from a reviewer with write access;
- stale approvals are dismissed when reviewable changes are pushed;
- all review conversations are resolved before merge;
- force pushes are blocked;
- deletion of `main` is restricted;
- linear history is required;
- squash merge is the accepted merge method;
- permanent bypass actors are not configured;
- required CI checks are added after the workflows have produced their stable check names.

Local hooks improve feedback speed but do not replace this server-side policy. Hooks can be skipped locally, while the GitHub ruleset and CI remain authoritative.

### Branches

Create focused branches from current `main`:

```text
docs/architecture
feat/dataset-manifest
feat/bybit-connector
feat/research-api
feat/report-dashboard
analysis/pump-reversion
fix/duplicate-observations
```

Avoid combining unrelated infrastructure, research, and interface changes in one branch.

### Commits

Commits should describe one coherent change and remain reviewable.

Suggested prefixes:

```text
feat:
fix:
analysis:
docs:
test:
refactor:
build:
ci:
```

Do not commit generated caches, local IDE state, credentials, private exports, or unrelated formatting changes.

### Pull requests

A pull request includes:

- problem and scope;
- architectural or methodological approach;
- commands and tests executed;
- data, schema, migration, and security impact;
- screenshots or generated artifacts for interface and report changes;
- limitations and follow-up work;
- linked issue or milestone.

Review is based on evidence. A change is not ready because it works once on one local dataset.

### Updating a branch

Before final review:

```bash
git fetch origin
git rebase origin/main
make check
```

Force-push is used only for an owned feature branch after confirming that no other contributor depends on its current history.

### Local Git hooks

The Python foundation will add `pre-commit` as a locked development dependency and commit `.pre-commit-config.yaml`.

The pre-commit stage runs fast checks suitable for every commit:

- trailing-whitespace and end-of-file normalization;
- YAML, JSON, and TOML syntax checks;
- merge-conflict marker detection;
- accidental large-file detection;
- private-key detection;
- Ruff linting and formatting on changed Python files;
- repository-specific lightweight contract checks when available.

The pre-push stage runs the complete implemented verification surface through `make check`, including formatting verification, lint, types, and tests. Longer integration or service tests remain in CI if they would make every push impractical.

Installation is exposed through a repository command rather than undocumented local setup:

```bash
make hooks-install
```

The implementation is expected to install both stages:

```bash
uv run pre-commit install --hook-type pre-commit --hook-type pre-push
```

Manual verification remains available:

```bash
uv run pre-commit run --all-files
make check
```

Using `git commit --no-verify` or `git push --no-verify` is reserved for diagnosing hook failures. It does not bypass required pull-request review or CI on `main`.

## Generated component workflows

The project will provide bounded generators for repeated structures.

### Connector

```bash
make new-connector NAME=<source>
```

Expected output:

- connector module;
- configuration model;
- normalized output contract reference;
- source fixture directory;
- unit and contract test skeleton;
- documentation stub for licensing, timestamps, pagination, and rate limits.

### Research module

```bash
make new-study NAME=<study>
```

Expected output:

- versioned study definition;
- configuration model;
- eligibility function;
- result contract;
- deterministic fixture;
- test skeleton;
- report and interpretation stub.

### Report

```bash
make new-report NAME=<report>
```

Expected output:

- report entry point;
- canonical result JSON model;
- Markdown or HTML renderer;
- figure directory;
- registry integration;
- snapshot or contract tests.

Generators create a consistent starting point. They do not determine research policy or remove the need for review.

## Local services

Docker Compose is introduced after a service requires Postgres, a queue, or an S3-compatible object-storage emulator. The first public-data and research milestones run through local Python commands over bounded files.

Profiles will keep the default development footprint bounded:

```text
core        Postgres and required metadata services
api         FastAPI
worker      background processing
web         React development or production build
full        complete local stack
```

## Database migrations

Metadata schema changes require a migration, upgrade test, downgrade test where supported, and compatibility review with currently registered datasets and reports.

Migrations must not rewrite or silently reinterpret immutable analytical datasets. Dataset schema evolution is handled through versioned contracts and explicit transformations.

## Testing strategy

### Unit tests

Pure parsing, normalization, validation, eligibility, metrics, and rendering behavior.

### Contract tests

Canonical schemas, connector fixtures, manifest compatibility, OpenAPI generation, and Python-to-TypeScript boundaries.

### Integration tests

Postgres repositories, queue lifecycle, object storage, FastAPI routes, and complete sample-data jobs.

### Regression tests

Every fixed production or source-integration defect receives the smallest deterministic reproduction that would fail without the correction.

### Interface tests

Component behavior, API states, responsive layout, accessibility, and selected end-to-end report flows.

## Secrets and local configuration

Commit `.env.example` with names and safe descriptions, never real values. Local `.env` files, credentials, private keys, IDE state, tool state, raw data, and complete generated datasets remain ignored.

The web application never receives source API secrets. Connector credentials are available only to the worker process responsible for the source.

## Definition of Done

A change is complete when:

- its public contract and scope are clear;
- implementation and tests are present;
- `make check` passes;
- schema and migration impact is handled;
- security and data implications are documented;
- operational telemetry is added for new long-running behavior;
- user-visible behavior has an artifact or screenshot;
- documentation matches the implemented command surface.
