# Data contracts

## Purpose

Data contracts make analytical inputs independently identifiable, verifiable, and reproducible across connectors, processing jobs, research modules, and report versions.

## Dataset package

An immutable dataset version is represented by a manifest and one or more data artifacts:

```text
<dataset-id>/<version>/
  manifest.json
  schema.json
  README.md
  data/
    part-*.parquet
  quality/
    summary.json
    checks.json
```

Large packages reside in object storage. Small reviewed samples may be committed under `data/sample/` with the same contract.

## Required manifest fields

```json
{
  "dataset_id": "market-events",
  "dataset_version": "v1",
  "schema_version": 1,
  "generated_at": "2026-01-01T00:00:00Z",
  "window_start": "2025-01-01T00:00:00Z",
  "window_end": "2026-01-01T00:00:00Z",
  "sources": [],
  "source_versions": {},
  "row_counts": {},
  "partitions": [],
  "checksums": {},
  "code_revision": "git revision",
  "censoring_policy": "documented policy",
  "license": "source-specific",
  "redistribution": "allowed, restricted, or prohibited",
  "limitations": []
}
```

The machine-readable schema defines exact types and validation constraints. The example documents the minimum semantic surface and is not the final JSON Schema.

## Timestamp contract

When the source permits, event-oriented data preserves four distinct timestamps:

- `occurred_at`: when the underlying event occurred;
- `published_at`: when the source published the event;
- `first_observed_at`: when the collector first observed it;
- `ingested_at`: when the platform durably accepted it.

Market observations additionally preserve source exchange time and local receive time where available. Timestamps are stored in UTC with explicit units. Missing source timestamps remain null and are not replaced by ingestion time.

## Identity contract

Canonical instrument identity is separate from source symbols.

```text
source
venue
market type
source symbol
base asset
quote asset
settlement asset
contract identifier
canonical instrument identifier
effective time range
```

Identity changes require evidence from an official source or an approved reference mapping. Symbol similarity is not sufficient evidence.

## Provenance

Every source record or partition must be traceable to:

- source adapter and version;
- endpoint or archive family;
- bounded request parameters;
- pagination or stream window;
- retrieval timestamp;
- raw-response checksum or approved source reference;
- normalization code revision;
- validation result.

Credentials, signed requests, and private response fields are excluded from published provenance.

## Quality states

Validation produces explicit states rather than one boolean:

```text
valid
partial
right_censored
left_censored
stale
duplicate
identity_unresolved
source_unavailable
license_restricted
invalid
```

Research modules declare which states are eligible. They may not silently coerce an ineligible state into a valid zero.

## Schema evolution

- Additive optional fields may retain the same major dataset family with a new schema version.
- Renamed fields, unit changes, identity changes, and semantic reinterpretation require an explicit migration or new dataset version.
- Historical data is never silently rewritten under the same manifest fingerprint.
- Consumers fail closed on unknown required schema versions.
- Transformations record input and output fingerprints.

## Licensing and redistribution

Each source records acquisition terms, storage permission, redistribution permission, attribution, retention limits, and restrictions on derived artifacts.

If raw redistribution is prohibited, the repository may still publish code, schemas, synthetic fixtures, permitted aggregates, and reproduction instructions. The manifest must not claim public availability for restricted artifacts.

## Schurfer export boundary

Sanitized Schurfer exports are immutable research inputs and follow the same manifest, schema, checksum, quality, and lineage requirements as public sources.

An export must exclude credentials, private configuration, direct database identifiers where unnecessary, unrestricted JSON context, private communication content, and any order-control capability.

## Reproducibility fingerprint

A registered research run identifies at least:

```text
dataset versions
dataset checksums
study version
configuration
code revision
dependency lock revision
time window
eligibility policy
random seed when applicable
```

Equivalent fingerprints are expected to produce equivalent deterministic outputs within documented numerical tolerances.
