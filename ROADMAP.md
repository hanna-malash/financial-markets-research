# Roadmap

The roadmap is organized around complete evidence-producing vertical slices. The target architecture is introduced incrementally, and infrastructure is added only when a working research flow establishes its contract or operating requirement.

## Milestone 0: repository and Python foundation

- [ ] Select and add the repository code license.
- [ ] Keep dataset licensing and redistribution policy separate from the code license.
- [ ] Generate one `uv` Python package and commit the lockfile.
- [ ] Add Ruff, mypy, pytest, and root Makefile commands.
- [ ] Add locked `pre-commit` tooling with fast commit hooks, a complete pre-push `make check`, and `make hooks-install`.
- [ ] Add pull-request CI using the same local commands.
- [ ] Configure an active GitHub ruleset protecting `main` from direct unreviewed changes, deletion, force pushes, and non-linear history.
- [ ] Require one approval and resolved review conversations before merge.
- [ ] Add stable CI check names to the ruleset after the first successful workflow run.
- [ ] Define the first dataset-manifest schema and deterministic fixture.
- [ ] Add `.env.example`, secret policy, and reviewed data-directory rules.
- [ ] Keep documentation consistent with the implemented command surface.

**Gate:** a clean checkout can bootstrap, install hooks, and pass formatting, lint, types, tests, and manifest validation through documented commands; `main` rejects direct unreviewed changes and cannot be force-pushed or deleted through the normal workflow.

## Milestone 1: first public-data research result

- [ ] Implement one bounded public market-data connector.
- [ ] Preserve source version, request bounds, UTC timestamps, and provenance.
- [ ] Normalize one dataset into a versioned canonical schema.
- [ ] Write deterministic Parquet with checksums and a manifest.
- [ ] Query the dataset through DuckDB without loading it fully into Python memory.
- [ ] Generate a machine-readable and human-readable data-quality report.
- [ ] Define and execute one event study with explicit eligibility, horizons, censoring, and uncertainty.
- [ ] Publish a static JSON and Markdown or HTML report with figures and limitations.

**Gate:** one real research question is answered reproducibly from acquisition through a registered static artifact without requiring FastAPI, React, Postgres, or a queue.

## Milestone 2: report contracts and deterministic automation

- [ ] Stabilize canonical result JSON from the first working report.
- [ ] Record dataset version, code revision, configuration, fingerprints, readiness, and interpretation.
- [ ] Add reusable report and figure generation outside notebooks.
- [ ] Add bounded scheduled local execution where useful.
- [ ] Add generators for new connectors, studies, and reports only after repeated structure is demonstrated.
- [ ] Define metadata projections required by later API consumers.

**Gate:** the report can be regenerated deterministically, compared across runs, and consumed without parsing presentation text.

## Milestone 3: read-only research API

- [ ] Generate the FastAPI application with a pinned toolchain.
- [ ] Expose health, datasets, data quality, studies, and report resources.
- [ ] Serve existing canonical report data rather than recomputing analytics in request handlers.
- [ ] Publish and validate the OpenAPI contract.
- [ ] Add authentication and artifact-access policy where required.
- [ ] Add API tests and bounded operational telemetry.

**Gate:** the first real dataset and report are available through a tested read-only API with stable identifiers and explicit incomplete or unavailable states.

## Milestone 4: research web application

- [ ] Generate the React TypeScript application with a pinned toolchain.
- [ ] Generate the TypeScript client from FastAPI OpenAPI.
- [ ] Display dataset catalog, coverage, quality, and freshness.
- [ ] Display study definition, readiness, results, uncertainty, costs, and limitations.
- [ ] Add reusable event-horizon, confidence-interval, concentration, cost, and drawdown charts as required by real reports.
- [ ] Add responsive, accessible, loading, stale, empty, failed, and partial states.

**Gate:** the first report can be inspected end to end without duplicating research calculations in the browser.

## Milestone 5: asynchronous jobs and durable metadata

- [ ] Register the measured need for scheduled execution, concurrency, retries, cancellation, recovery, or interactive progress.
- [ ] Select and document Postgres, queue, and object-storage responsibilities.
- [ ] Add durable job identity and valid state transitions.
- [ ] Execute acquisition, validation, research, and report jobs through a worker.
- [ ] Emit bounded progress and structured failures.
- [ ] Add idempotency, retry classification, cancellation, and stale-job recovery.
- [ ] Display job state through FastAPI and React.

**Gate:** repeated requests cannot duplicate immutable outputs, failed jobs remain diagnosable, and partial results cannot be presented as complete.

## Milestone 6: Schurfer research exports

**External dependency:** blocked until Schurfer registers and implements `research-export-v1` with an owner, schema, sanitization policy, tests, and publication approval.

- [ ] Link the registered Schurfer deliverable and approved export version.
- [ ] Validate export identity, timestamps, schema, checksums, and exclusions.
- [ ] Register the export as an immutable source dataset.
- [ ] Reproduce one event-level result independently.
- [ ] Add aggregated order-flow windows only after their contract and retention are approved.
- [ ] Keep production credentials, private context, direct database access, and trading control outside this platform.

**Gate:** Schurfer-derived research is reproducible without production connectivity and contains no private control surface or unreviewed data.

## Milestone 7: market microstructure and order flow

- [ ] Add aggregated public-trade and order-flow contracts.
- [ ] Analyze pre-event imbalance, continuation risk, and delayed reversal as separate studies.
- [ ] Add cross-venue identity and timestamp alignment.
- [ ] Measure lead-lag behavior, latency decay, and control matching.
- [ ] Visualize event-relative order flow, price, liquidity, and outcome paths.

**Gate:** each research lane has independent eligibility, outcome, costs, uncertainty, and interpretation rather than one combined headline.

## Milestone 8: source expansion

- [ ] Add macroeconomic or regulatory data with publication-time semantics.
- [ ] Add listings, delistings, or corporate-action events from official sources.
- [ ] Add approved news or event metadata only after licensing review.
- [ ] Add source freshness, revision, and outage diagnostics.
- [ ] Add cross-asset reference identity and calendars.

**Gate:** every source has a documented license, timestamp model, pagination policy, quality report, and bounded connector implementation.

## Milestone 9: portfolio and risk analytics

- [ ] Add benchmarks and return conventions across asset classes.
- [ ] Add volatility, correlation, drawdown, turnover, and allocation modules.
- [ ] Add risk decomposition and regime segmentation.
- [ ] Compare simple allocation baselines before model-driven policies.
- [ ] Visualize exposure, contribution, turnover, and drawdown with complete units and periods.

**Gate:** results are point-in-time, cost-aware, benchmarked, and reproducible across declared rebalance schedules.

## Milestone 10: operational hardening

- [ ] Add source, queue, worker, API, storage, and report telemetry as their components are introduced.
- [ ] Add retention and storage-budget enforcement.
- [ ] Add backup and restore tests for metadata and manifests.
- [ ] Add access control and artifact-publication policy.
- [ ] Add dependency, container, secret, and migration checks.
- [ ] Measure workloads before selecting ClickHouse, distributed processing, or additional service separation.

**Gate:** the platform fails visibly, preserves lineage, respects storage and security budgets, and can recover its metadata state from tested backups.
