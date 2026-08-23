# Research methodology

## Research lifecycle

```text
question
  -> data and timestamp contract
  -> population and eligibility
  -> outcome and cost definition
  -> discovery analysis
  -> robustness diagnostics
  -> frozen validation contract
  -> fresh or held-out evidence
  -> publication, revision, or rejection
```

## Research question

A research definition identifies:

- event or decision unit;
- eligible population;
- observation and entry time;
- outcome horizons;
- direction and return convention;
- benchmark or matched control;
- execution and cost assumptions;
- clustering unit;
- minimum sample, diversity, and time coverage;
- invalidation and missing-data rules.

## Point-in-time discipline

Features must be available at the simulated decision time. Publication, first-observation, ingestion, and source-event timestamps remain distinct. Revised macro data, updated classifications, future liquidity, final wallet labels, and post-event membership cannot be used as if they were known earlier.

## Censoring

Right-censored observations have not had enough future time to mature. Left-censored observations lack the required pre-event window. Neither state is a loss, win, zero, or valid control.

Reports show eligible, complete, censored, unavailable, and excluded counts separately.

## Event independence and clustering

Repeated decisions or alerts from the same underlying event do not automatically create independent observations. Event-level selection is defined explicitly. Uncertainty is clustered by the appropriate market unit, such as asset, event, venue, date, or regime.

Reports include:

- observation count;
- independent event count;
- cluster count;
- largest-cluster share;
- time coverage;
- leave-one-cluster-out sensitivity when concentration is material.

## Controls and benchmarks

Matched controls use information available before the event. Matching variables, exclusion rules, and reuse policy are registered. A symbol currently experiencing the target event cannot be used as an unaffected control.

Simple baselines remain visible alongside complex models. Improvements are measured on matched populations whenever possible.

## Costs and execution

Tradability analysis distinguishes:

- gross market move;
- entry and exit fees;
- spread;
- market impact;
- observed executable quote versus actual fill;
- funding credit or cost;
- latency and signal decay;
- unfilled or rejected orders;
- adverse selection;
- protective stop execution;
- failed or delayed exits.

Fixed-horizon returns describe market behavior and do not substitute for a path-dependent execution replay.

## Discovery and validation

Discovery may compare multiple thresholds, horizons, features, or policies to generate hypotheses. Discovery output is labeled and cannot directly promote a production strategy.

Validation freezes the cohort start, eligibility, metric, comparator, sample gates, and interpretation before reading mature outcomes. Changes after registration create a new contract rather than silently modifying the existing one.

## Uncertainty and robustness

Depending on the study, reports may include:

- mean and median;
- quantiles and tail outcomes;
- confidence intervals;
- clustered bootstrap;
- profit factor and expectancy;
- drawdown and turnover;
- sensitivity to fees, spread, impact, and delay;
- liquidity, venue, regime, and magnitude segments;
- leave-one-cluster-out analysis;
- multiple-testing controls when many hypotheses are screened.

Statistical significance is not sufficient evidence of economic significance, capacity, or implementability.

## Machine learning

Machine-learning models are introduced only after a reproducible baseline and adequate point-in-time data exist.

Evaluation requires:

- time-based train, validation, and test partitions;
- leakage-safe feature generation;
- asset or cluster separation where appropriate;
- purging or embargo for overlapping horizons;
- probability calibration for probabilistic outputs;
- comparison with simple rules;
- costs and achievable latency;
- feature stability and missingness diagnostics;
- held-out or prospective evaluation.

Model complexity is justified by out-of-sample improvement, not in-sample fit.

## Published report contract

A published report includes:

- study and dataset versions;
- generated time and code revision;
- input fingerprint;
- population and exclusions;
- readiness and completeness state;
- headline metrics with units;
- uncertainty and concentration;
- robustness diagnostics;
- figures derived from canonical result data;
- limitations;
- interpretation state;
- exact reproduction command.

Negative and inconclusive results remain registered when they close a documented hypothesis or prevent repeated testing of the same idea without new evidence.
