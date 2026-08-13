# Workflow overview

MedDeID workflows are composed from independent tools that exchange canonical JSONL and checksum-bearing manifests. Choose only the stages your project needs.

## Clinical inference

```text
note.txt or documents.jsonl
  → meddeid
  → de-identified text or predictions.jsonl
```

Use this path when a trained model already meets the needs of the target setting. [Run local inference](inference.md).

## Dataset preparation and review

```text
hospital TXT / CSV / TSV / Parquet
  → meddeid-data project
  → optional model pre-annotations
  → meddeid-annotate
  → completed canonical annotations
```

One completed primary annotation is technically sufficient for training. Add another reviewer and `meddeid-curate` only when your protocol requires independent review. [Prepare and annotate data](prepare-and-annotate.md).

## Gold benchmark

```text
completed primary annotations
  → optional meddeid-curate
  → meddeid-subannotate
  → checksummed benchmark bundle
  → meddeid-eval
```

Subannotation records which characters inside a primary gold span count toward core-PII recall. It is an evaluation workflow, not a prerequisite for training.

## Model adaptation

```text
reviewed development data + sealed test gold
  → meddeid-data prepare-training
  → meddeid-training
  → exported model bundle
  → meddeid batch
  → meddeid-eval
```

Use the two-stage selection/refit protocol for a publication release. A one-time `fit` is available for ordinary research experiments. [Train and evaluate](train-and-evaluate.md).

## Invariants across workflows

All supported paths preserve these rules:

- offsets are half-open `[begin, end)` Unicode code-point positions;
- `document_id` identifies a document within a dataset revision;
- `spans` is the only canonical primary-span container;
- inputs are not silently overwritten;
- manifests pin lineage with SHA-256 hashes;
- the independent test set is not used for model selection.
