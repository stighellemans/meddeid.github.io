# Choose your path

MedDeID is a suite, but most users need only a small part of it. Start from the outcome you want.

For inference, install `meddeid` from PyPI or use the published GHCR image.
Data, training, evaluation, and browser applications currently use their
individual public source repositories; their workflow pages include the exact
installation route.

| Your goal | Begin with | Add only when needed |
|---|---|---|
| De-identify one note or a batch of notes | `meddeid` | `meddeid[server]` for an internal web service |
| Import hospital data for review | `meddeid-data` | `meddeid` for model pre-annotations |
| Review and correct PII spans with one annotator | `meddeid-annotate` | `meddeid` for optional model pre-annotations |
| Reconcile multiple reviewers | `meddeid-curate` | Use only when the study protocol requires it |
| Create a detailed evaluation benchmark | `meddeid-subannotate` | Start from completed reviewed annotations |
| Train or adapt a model | `meddeid-training[train]` | `meddeid-data` to organize training and test data |
| Score predictions or test stability | `meddeid-eval` | `meddeid-eval[plots]` for figures |
| Add support for another language | `meddeid-core` | Implement a separate `meddeid-language-*` package and model bundle |

## Common paths

=== "Inference only"

    ```text
    text or a batch of notes → meddeid → redacted text + detected identifiers
    ```

    Continue to [local inference](../workflows/inference.md).

=== "Research dataset"

    ```text
    source notes → meddeid-data → meddeid-annotate
      → optional meddeid-curate → reviewed primary annotations
    ```

    Continue to [prepare and annotate data](../workflows/prepare-and-annotate.md).

=== "Domain adaptation"

    ```text
    reviewed development data → meddeid-training → adapted bundle
      → meddeid batch → meddeid-eval
    ```

    Continue to [domain adaptation](../workflows/domain-adaptation.md).

## What you do not need

- You do not need the grouped suite workspace. PyPI users install released
  inference packages directly; source-only users clone only the component they
  need.
- You do not need training, evaluation, or annotation packages for ordinary inference.
- You do not need curation for a completed single-reviewer dataset.
- You do not need detailed character-level benchmark labels for ordinary model training.
