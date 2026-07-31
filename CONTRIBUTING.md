# Contributing

## Scope

Contributions should improve data contracts, source integrations, validation, reproducibility, research correctness, automation, reporting, visualization, security, or operational reliability.

## Before implementation

- Link the change to a tracked issue or roadmap milestone.
- State the problem, proposed scope, inputs, outputs, and verification plan.
- Confirm licensing and timestamp semantics for new data sources.
- Identify schema, migration, storage, security, and compatibility impact.
- Keep unrelated concerns in separate changes.

## Branches

Create a focused branch from current `main`:

```text
docs/data-contracts
feat/dataset-catalog
feat/bybit-connector
feat/research-api
feat/report-dashboard
analysis/pump-reversion
fix/duplicate-observations
```

Do not commit directly to `main`.

## Pull requests

Every pull request should include:

- problem and scope;
- implementation or research approach;
- verification commands and results;
- data and schema impact;
- security and licensing impact;
- generated artifacts or screenshots where relevant;
- limitations and follow-up work;
- linked issue or milestone.

Pull requests should remain atomic enough to review, test, revert, and release independently.

## Code quality

- Use repository package managers and committed lockfiles.
- Keep public contracts typed and versioned.
- Add deterministic tests for parsing, normalization, validation, and calculations.
- Add regression tests for corrected defects.
- Keep network calls outside pure analytical functions.
- Keep statistical calculations outside frontend code.
- Avoid loading complete datasets into memory when projection or streaming is available.
- Emit structured, bounded operational telemetry for long-running behavior.

## Data rules

- Never commit credentials, tokens, `.env` files, private keys, or private configuration.
- Never commit an unreviewed production export.
- Keep complete raw, intermediate, and processed datasets outside Git.
- Commit only small, licensed, documented examples under `data/sample/`.
- Preserve source provenance and point-in-time timestamps.
- Represent missing, unavailable, and censored outcomes explicitly.
- Document redistribution policy for every external source.

## Notebooks

Notebooks may be used for exploration and presentation. Reusable acquisition, validation, feature, cohort, metric, and reporting logic belongs in typed packages with tests. A notebook must not be the only implementation of a published number.

## Research changes

A research pull request identifies population, eligibility, event time, horizons, outcome convention, costs, controls, clustering, completeness, and interpretation state.

Discovery output cannot silently replace a frozen validation contract. Changes to a registered contract produce a new version.

## Review

Review focuses on contract correctness, failure behavior, data leakage, reproducibility, resource bounds, security, and interpretation. Review comments should be resolved with code, tests, documentation, or explicit evidence.

## Merge requirements

- Required CI passes.
- Generated contracts and clients are current.
- Database migrations and dataset schema changes are reviewed.
- Documentation matches implemented behavior.
- No secrets or unapproved data artifacts are present.
- The change has an observable result or explicit verification artifact.
