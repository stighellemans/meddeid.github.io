# Public artifacts

The [MedDeID Hugging Face collection](https://huggingface.co/collections/stighellemans/meddeid) contains the current default model and datasets. Each item has its own page with usage guidance, limitations, licence, and version history.

## Model

### `stighellemans/meddeid-dutch-synth`

The current Dutch clinical de-identification model, trained on synthetic data. Download and run the complete model through the MedDeID package:

```bash
pip install meddeid
meddeid deidentify note.txt
```

For a reproducible or air-gapped deployment, save the exact model version and keep its files together.

## Datasets

### `stighellemans/meddeid-dutch-synthetic-corpus`

The complete 6,493-document synthetic dataset used for model development. Divide this dataset into training and validation data; keep the independent benchmark below separate.

### `stighellemans/meddeid-dutch-synthetic-benchmark`

An independent set of 300 synthetic documents for final testing. It includes extra reviewed detail for measuring whether the important parts of each identifier were removed.

## Annotation guidelines

The current `ProductionLabels_v1` guidelines are available in English and Dutch. PDF reading copies and editable DOCX sources are included in the [Zenodo release](https://doi.org/10.5281/zenodo.21890965). They explain what annotators should mark; the precise software rules are documented separately in `meddeid-core`.

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

The [MedDeID hosted demo](https://huggingface.co/spaces/stighellemans/meddeid-demo)
runs the public synthetic model for non-sensitive examples. It executes on
Hugging Face infrastructure: never paste real patient or caregiver information.
Use `meddeid` locally for clinical text.
