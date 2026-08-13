# Train and evaluate

Use reviewed development data to fit a model. Keep an independent test set sealed until the final evaluation.

## Prepare split-safe views

<span class="source-label">Owner: meddeid-data</span>

```bash
meddeid-data project prepare-training my-project \
  --development assignments/development-reviewer-a.jsonl \
  --test-gold evaluation/meddeid-dutch-synthetic-benchmark.jsonl
```

This validates completion, document membership, text identity, labels, spans, and lineage before writing three views:

| View | Purpose |
|---|---|
| `prepared/fit` | One ordinary train/validation/test experiment |
| `prepared/selection` | Select an epoch count without access to test gold |
| `prepared/refit` | Recombine development data, refit, and evaluate once on sealed test gold |

## Ordinary research fit

<span class="source-label">Owner: meddeid-training</span>

```bash
pip install 'meddeid-training[train]'

meddeid-train fit \
  --config configs/release.yaml \
  --data prepared/fit \
  --run runs/fit
```

Validation chooses the best checkpoint. Test evaluation occurs after fitting. The result is `runs/fit/checkpoints/best.pt`.

## Publication protocol

For a release-quality experiment, separate epoch selection from full-development-data refitting:

```bash
meddeid-train select-epochs \
  --config configs/release.yaml \
  --data prepared/selection \
  --run runs/selection

meddeid-train refit \
  --config configs/release.yaml \
  --selection runs/selection/run.json \
  --data prepared/refit \
  --run runs/refit

meddeid-train export \
  --checkpoint runs/refit/checkpoints/best.pt \
  --run-metadata runs/refit/train_metrics.json \
  --output release/my-model
```

Selection and refit restart independently from the configured initial model. Refit does not continue from the selection checkpoint. The exported directory is a self-contained bundle for `meddeid`.

## Evaluate predictions

<span class="source-label">Owner: meddeid-eval</span>

Generate predictions with the exact exported bundle:

```bash
meddeid batch prepared/refit/test.jsonl \
  --model release/my-model \
  --output predictions/test.jsonl
```

Then score the canonical prediction file:

```bash
meddeid-eval score \
  --gold prepared/refit/test.jsonl \
  --predictions predictions/test.jsonl
```

The report includes exact-span precision, recall, and F1; character coverage; core-PII recall when reviewed subannotations are present; and non-PII redaction rate.

External comparators follow the same boundary: run them independently, convert their outputs to canonical prediction JSONL, and pass that file to `meddeid-eval`. They are not imported into the MedDeID runtime.

## Record with every result

- hashes of train, validation, and test manifests;
- initial model repository and immutable revision;
- language profile identity and resource hashes;
- ordered model labels and taxonomy version;
- resolved configuration and random seeds;
- package versions and hardware/runtime information;
- exported model checksums;
- exact prediction and metric commands.

This information belongs in machine-readable run and output manifests. The paper or report should link to those records rather than reproduce them manually.
