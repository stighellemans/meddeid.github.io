# Install and run

## Requirements

- Python 3.10 or later
- **CPU:** 8 GiB RAM recommended
- **Optional NVIDIA GPU:** at least 8 GiB VRAM
- About 1 GB free disk space and internet access for the first model download

These are practical starting points; production needs depend on workload.

## De-identify a text file

```bash
pip install meddeid
meddeid deidentify note.txt
```

MedDeID downloads and caches `stighellemans/meddeid-dutch-synth` on first use, chooses a local device, and processes the note locally.

## Use the Python API

```python
from meddeid import Deidentifier

deidentifier = Deidentifier.from_pretrained(
    "stighellemans/meddeid-dutch-synth"
)
result = deidentifier("Patiënt Alex Voorbeeld kwam op controle.")

print(result.deid_text)
print(result.spans)
deidentifier.close()
```

## Process canonical JSONL

```bash
meddeid batch documents.jsonl --output predictions.jsonl
```

The batch command preserves document IDs and order and writes a sidecar manifest describing inputs, model identity, runtime, language profile, and timing.

## Confirm what ran

```bash
meddeid model-info
```

Record the immutable model revision shown by this command when the result must be reproducible.

!!! tip "Air-gapped environments"
    Download an immutable model snapshot outside the secure environment, validate it, transfer it according to local policy, and pass the local directory with `--model`. See [local inference](../workflows/inference.md#offline-and-air-gapped-use).

## Next steps

- [Run batch or service inference](../workflows/inference.md)
- [Understand the JSONL contract](../concepts/data-contract.md)
- [Review privacy and security boundaries](../project/privacy-and-security.md)
