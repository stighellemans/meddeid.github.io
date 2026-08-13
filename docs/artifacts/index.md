# Public artifacts

The [MedDeID Hugging Face collection](https://huggingface.co/collections/stighellemans/meddeid) groups the current default model and datasets. Each artifact has its own card, licence, checksums, and revision history.

## Model

### `stighellemans/meddeid-dutch-synth`

The default Dutch clinical de-identification model, trained on the synthetic development corpus. The bundle includes weights, tokenizer, encoder configuration, ordered labels, inference settings, and reproducibility metadata.

Use it through the package rather than downloading individual files:

```bash
pip install meddeid
meddeid deidentify note.txt
```

For a reproducible or air-gapped deployment, pin an immutable Hub revision and retain the complete bundle.

## Datasets

### `stighellemans/meddeid-dutch-synthetic-corpus`

The complete 6,493-document synthetic model-development corpus. Its published split is `train`; users create their own development/validation partition without treating the independent benchmark as validation data.

### `stighellemans/meddeid-dutch-synthetic-benchmark`

An independent 300-document synthetic benchmark published as `test`. Reviewed core-PII character subannotations are nested under their primary gold spans for detailed recall measurement.

## Annotation guidelines

The current `ProductionLabels_v1` guidelines are published in English and Dutch, as stable PDF reading copies and editable DOCX sources in the [Zenodo release](https://doi.org/10.5281/zenodo.21890965). The guidelines define annotation intent; `meddeid-core` defines the machine-readable taxonomy and validation contract.

## Artifact documentation belongs with the artifact

Model and dataset cards are authoritative for:

- exact files, row counts, and checksums;
- intended use and out-of-scope use;
- training or generation provenance;
- evaluation results and limitations;
- licences and required attribution;
- immutable revisions and citations.

This site explains how artifacts fit into suite workflows. It should not copy full cards, because copied limitations and revisions become stale.

## Public demo

A public hosted demo is still under consideration and is not currently part of the release. For now, use `meddeid` locally, including for non-sensitive examples.
