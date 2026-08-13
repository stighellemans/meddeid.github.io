# Local inference

<span class="source-label">Owner: meddeid</span>

`meddeid` is the only required package for normal inference. It contains the Python API, CLI, batch runner, decoder, and optional HTTP service.

## One note

```bash
meddeid deidentify note.txt
```

The command uses the published Dutch synthetic model by default. The model is cached locally after its first download; the note itself is not sent to Hugging Face.

## A canonical batch

```bash
meddeid batch project/splits/test.jsonl \
  --output predictions/test.jsonl
```

The output uses the same `document_id`, `text`, and `spans` contract accepted by `meddeid-eval` and the annotation applications. A sidecar manifest captures model, revision, device, runtime, language profile, package versions, checksums, and timing.

Use `--revision <immutable-hub-sha>` for a pinned study. Use `--device cpu`, `--device mps`, or `--device cuda` only when automatic selection is inappropriate.

## Python

```python
from meddeid import Deidentifier

with Deidentifier.from_pretrained(
    "stighellemans/meddeid-dutch-synth",
    revision="<immutable-hub-sha>",
) as deidentifier:
    result = deidentifier(
        "Patiënt Alex Voorbeeld kwam op controle.",
        metadata={"patient": {"given_name": "Alex", "family_name": "Voorbeeld"}},
    )
```

Trusted metadata can help recover known patient or caregiver names after neural inference. It is not concatenated to the note or used as a model input. Incorrect metadata can cause false-positive redaction, so validate its origin and shape.

## HTTP service

```bash
pip install 'meddeid[server]'
MEDDEID_DEVICE=cpu meddeid-server
```

The service exposes:

- `POST /deidentify` for one document;
- `POST /deidentify-batch` for throughput-oriented batches;
- `GET /health` for model identity and readiness.

The service does not remove the need for network authentication, TLS, authorization, request limits, and safe logging. Put those controls at the deployment boundary.

## Offline and air-gapped use

Download an immutable snapshot before entering the air-gapped environment:

```bash
hf download stighellemans/meddeid-dutch-synth \
  --revision <immutable-hub-sha> \
  --local-dir ./meddeid-dutch-synth

meddeid deidentify note.txt --model ./meddeid-dutch-synth
```

Transfer and validate the complete directory. Do not copy only the weights: the tokenizer, bundle contract, encoder configuration, ordered labels, language profile identity, and checksums are part of the model artifact.

## Production checklist

- Pin an immutable model revision or local artifact hash.
- Validate recall and unnecessary redaction on representative local data.
- Keep source notes, predictions, manifests, caches, and logs inside the approved boundary.
- Configure concurrency only after measuring memory and latency on target hardware.
- Monitor model identity and health, not patient content.
- Define a human-review path for high-consequence use.

The component repository remains authoritative for every CLI switch, server setting, and backend option.
