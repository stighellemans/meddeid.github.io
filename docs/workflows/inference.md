# Local inference

<span class="source-label">Owner: meddeid</span>

`meddeid` is the package used to de-identify text. It supports individual notes,
batches, Python applications, and an optional internal web service. It has not
been released to PyPI yet.

## Availability today

The public model and source repositories are available. Command-line, Python,
batch, and HTTP use work from source. A clean checkout also builds a pinned,
model-bundled CPU image and hardened local Compose service. PyPI and GHCR
publication await the first coordinated tag. GPU-optimized releases, a public
demo, and a managed endpoint are not available yet.

For the least technical path, install Docker and run this from the `meddeid`
repository:

```bash
./scripts/start-local.sh
```

This generates authentication, builds and starts the service, and prints the
single-note browser interface and technical API documentation addresses.

Install the currently verified source commits together:

```bash
python -m pip install \
  'meddeid-core @ git+https://github.com/stighellemans/meddeid-core.git@331ec3bd81dd0996241d8e2aad12671e05c7d0fb' \
  'meddeid-language-nl @ git+https://github.com/stighellemans/meddeid-language-nl.git@f8aefe071ede04fcefc0c9b124b34e17923cb31e' \
  'meddeid[server] @ git+https://github.com/stighellemans/meddeid.git@f12fdbc38bd7b2f6fb4dc6c540b769b66ea410fa'
```

The shorter `pip install 'meddeid[server]'` command is the release target, not
a currently working installation command. The component repository tracks the
remaining release work.

## One note

```bash
meddeid deidentify note.txt
```

The command uses the published Dutch synthetic model by default. The model is cached locally after its first download; the note itself is not sent to Hugging Face.

## A batch of notes

```bash
meddeid batch project/splits/test.jsonl \
  --output predictions/test.jsonl
```

The output can be opened directly in the annotation tools or evaluated with `meddeid-eval`. MedDeID also saves the model version, settings, and timing information needed to reproduce the run.

For a study, use `--revision` to keep the model version fixed. Use `--device cpu`, `--device mps`, or `--device cuda` only when automatic selection is inappropriate.

## Python

```python
from meddeid import Deidentifier

deidentifier = Deidentifier.from_pretrained(
    "stighellemans/meddeid-dutch-synth",
    revision="<immutable-hub-sha>",
)
result = deidentifier(
    "Patiënt Alex Voorbeeld kwam op controle.",
    metadata={"patient": {"given_name": "Alex", "family_name": "Voorbeeld"}},
)
deidentifier.close()
```

Trusted information already known by the hospital, such as a patient or caregiver name, can help catch identifiers the model missed. This information is not added to the model input. Incorrect values can cause unnecessary redaction, so validate them carefully.

## HTTP service

```bash
MEDDEID_DEVICE=cpu meddeid-server
```

The service exposes:

- `POST /deidentify` for one document;
- `POST /deidentify-batch` for throughput-oriented batches;
- `GET /health` for model identity and readiness.

Protect the service with authentication, encrypted connections, access controls, request limits, and safe logging before making it available on a network.

## Containers and GPU serving

The standalone Dockerfile pins the public source dependencies and model. It
does not rely on unpublished PyPI projects or a first-start model download.
Compose binds only to localhost and applies non-root, read-only,
capability-free, bounded-process, rotating-log, API-key, and health-check
defaults.

The GHCR image becomes available after the first reviewed release tag. A
production GPU deployment still needs a model build prepared and tested for its
specific target hardware.

## Offline and air-gapped use

Download a fixed model copy before entering the air-gapped environment:

```bash
hf download stighellemans/meddeid-dutch-synth \
  --revision <immutable-hub-sha> \
  --local-dir ./meddeid-dutch-synth

meddeid deidentify note.txt --model ./meddeid-dutch-synth
```

Transfer and validate the complete directory rather than copying only the model weights. The other files are also required for correct predictions.

## Production checklist

- Keep the exact model version fixed.
- Validate recall and unnecessary redaction on representative local data.
- Keep source notes, predictions, run records, caches, and logs inside the approved boundary.
- Configure concurrency only after measuring memory and latency on target hardware.
- Monitor model identity and health, not patient content.
- Define a human-review path for high-consequence use.

See the component repository for the complete command and server reference.
