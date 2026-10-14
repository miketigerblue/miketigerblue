# Migrating a threat intelligence lake without losing its history

**Mike Harris · Migration: 14–16 May 2026 · Written up: 14 October 2026**

I moved my independent threat intelligence lake off its original Fly.io database while preserving the historical analyses for comparison with the replacement platform. The result was a cold archive plus a queryable legacy schema alongside the newer system.

The migration also exposed problems that a successful restore alone would have missed: EPSS scores that were present but absent from a query-facing column, a foreign-key ordering bug that stopped enrichment, and historical analyses whose model metadata had never been recorded.

## The problem

The old system combined a Rust ingestor, a hosted PostgreSQL lake, an analyst application and downstream reporting. By May, the replacement stack used Go-based Tiger2Go for ingestion, Python Tiger-Eye for enrichment and PostgreSQL with pgvector. The migration notes identify T2 as the Go successor to the earlier Number 2 reporting service.

The old corpus contained **12,463 analyses**, produced between October 2025 and May 2026. Reprocessing them immediately would have changed the record of what the earlier system produced. Keeping them unchanged made later comparisons possible, including comparisons of errors and differences in classification. [Migration record](https://github.com/miketigerblue/tiger-eye/blob/018fe6f6f94203d316a6e17bf4a95a2bf4f1334a/docs/MIGRATION-2026-05.md)

## The decision: preserve an archive and keep the data queryable

Four options were considered:

| Option | Benefit | Trade-off |
|---|---|---|
| Cold archive only | Retains a recoverable copy | Every investigation starts with a restore |
| Keep a hosted read-only clone | Retains a query endpoint | Continues the hosting dependency and cost |
| Restore only into the new database | Makes historical comparisons easy | Leaves preservation dependent on that environment |
| **Cold archive plus a separate legacy schema** | **Combines preservation with live SQL comparisons** | **Requires both an archive check and a selective restore** |

I chose the fourth option. The migration record reports a **4,825 MB source database** packaged into a **441 MB archive**, with a SHA-256 check after upload. The threat intelligence tables were restored under `legacy.*`; application schemas outside that scope remained in the archive.

The checksum established the integrity of the archived bytes. It did not, by itself, establish that every restored object or downstream dependency behaved correctly. [Archive and restore record](https://github.com/miketigerblue/tiger-eye/blob/018fe6f6f94203d316a6e17bf4a95a2bf4f1334a/docs/MIGRATION-2026-05.md#4-what-was-preserved)

## The restore needed more than a database copy

The source and destination differed in PostgreSQL tooling, extensions and schema layout. The recorded work included selecting compatible restore tooling, filtering objects whose extensions were absent, and handling a server setting unsupported by the destination.

Schema names created a subtler problem. Moving application tables also changed references to extension-owned functions and types. A broad replacement could have moved those references into a schema where the extension objects did not exist. The recorded solution inspected extension membership and treated those references separately from application objects.

The notes record **24 minutes for the restore pipeline**. That is one historical execution time, not the duration of the whole migration or a recovery-time guarantee. The hosted database was retired on 16 May; its old endpoints ceased to be available. This was a deliberate cutover, not evidence of uninterrupted service for every consumer. [Execution record](https://github.com/miketigerblue/tiger-eye/blob/018fe6f6f94203d316a6e17bf4a95a2bf4f1334a/docs/MIGRATION-2026-05.md#5-execution-log)

## What comparison exposed

### Stored data was not reaching analyst queries

The daily partitions contained roughly 11 million EPSS records, yet the `epss` column in `cve_enriched` was null across 351,080 rows. A query filtering that column for elevated scores returned nothing.

The fix materialised the latest available score into the query-facing column. The recorded coverage became **333,455 of 351,080 rows, or 95.0%**. The function updated only changed values, and the pull request records an idempotency check.

The lesson is concrete: ingestion counts alone cannot establish that the fields analysts use are populated. [Tiger2Go PR #17](https://github.com/miketigerblue/tiger2go/pull/17)

### A telemetry relationship could stop useful work

Enrichment generated a run ID, persisted analyses referring to it, and inserted the parent run row only after the batch. A newly enforced foreign key rejected that order.

The fix created the run row before processing and finalised its statistics afterwards. When the initial telemetry insert failed, the implementation allowed enrichment to continue with a null run reference. This made the failure behaviour explicit: losing a telemetry link should not automatically prevent an analysis from being stored. [Tiger-Eye PR #37](https://github.com/miketigerblue/tiger-eye/pull/37)

### Missing history had to remain visibly missing

For **2,410 older analyses**, the original model ID and input hash were unknown. The backfill added a `legacy-pre-pr22` cohort marker to the prompt and pipeline version fields, while leaving the unknown model IDs and hashes null.

That made the older records selectable as a group without inventing execution history. It was cohort labelling, not recovery of complete provenance. [Tiger-Eye PR #38](https://github.com/miketigerblue/tiger-eye/pull/38) · [Backfill SQL](https://github.com/miketigerblue/tiger-eye/blob/018fe6f6f94203d316a6e17bf4a95a2bf4f1334a/scripts/backfill_provenance_legacy_cohort.sql)

## The outcome: a system that could be compared

The historical corpus remained queryable alongside the newer model. Three comparison views exposed actor representations, KEV fields and analyses of overlapping articles.

The analysis view matched **1,608 article GUIDs** at the time of the recorded snapshot. That provided a useful set for inspecting output differences. It was not a controlled accuracy evaluation: pipeline versions, context and analysis times could differ, and agreement between two outputs would not establish correctness.

Similarly, the actor comparison counted **7,412 legacy name strings** and **488 canonical entities** in the newer system. Those are different populations and representations; their ratio is not a measured reduction in false positives. [Tiger2Go PR #18](https://github.com/miketigerblue/tiger2go/pull/18)

Preserving the old data gave the replacement system something concrete to be tested against. The most useful result was the ability to find missing fields, inspect changed outputs and distinguish recorded facts from assumptions.

## Evidence and scope

This case study concerns my independent platform. It was reconstructed from the published migration notes, merged pull requests and the public cohort-backfill and comparison-view SQL. Historical sizes, counts and timings are reported from those records; the archive and restore were not rerun for this write-up.

[October production measurements](2026-10-07-13-enrichment.md) · [Back to my profile](../README.md)
