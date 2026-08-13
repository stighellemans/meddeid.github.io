# Choose your path

MedDeID is a suite, but most users need only a small part of it. Start from the outcome you want.

| Your goal | Begin with | Add only when needed |
|---|---|---|
| De-identify one note or a JSONL batch | `meddeid` | `meddeid[server]` for HTTP |
| Import hospital data for review | `meddeid-data` | `meddeid` for model pre-annotations |
| Review one annotation assignment | `meddeid-annotate` | Nothing else |
| Reconcile multiple reviewers | `meddeid-curate` | Use only when the study protocol requires it |
| Create a core-PII evaluation benchmark | `meddeid-subannotate` | Start from completed primary gold |
| Train or adapt a model | `meddeid-training[train]` | `meddeid-data` to prepare split-safe views |
| Score predictions or test stability | `meddeid-eval` | `meddeid-eval[plots]` for figures |
| Build another language profile | `meddeid-core` | Implement a separate language package |

## Common paths

=== "Inference only"

    ```text
    text or canonical JSONL → meddeid → redacted text + prediction JSONL
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

- You do not need the grouped suite workspace to run a released component.
- You do not need training, evaluation, or annotation packages for ordinary inference.
- You do not need curation for a completed single-reviewer dataset.
- You do not need core-PII subannotations for ordinary model training.
- You do not need Belgian DEDUCE; it is an external comparator, not a MedDeID dependency.
