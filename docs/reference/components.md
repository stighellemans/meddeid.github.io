# Components

Each component is independently packaged, versioned, tested, licensed, and released. The grouped suite workspace is a staging aid, not a runtime dependency.

<div class="component-grid" markdown>

<div class="component-card" markdown>
### `meddeid`

Python API, CLI, batch inference, decoder, local model loading, and optional HTTP service.

**Install:** `pip install meddeid`
</div>

<div class="component-card" markdown>
### `meddeid-core`

Language-neutral schema, taxonomy, ordered model labels, normalization, and offset validation.

**Install:** `pip install meddeid-core`
</div>

<div class="component-card" markdown>
### `meddeid-language-nl`

Dutch parsing and rendering rules, `nl-BE` post-processing, and versioned Belgian lookup resources.

**Install:** `pip install meddeid-language-nl`
</div>

<div class="component-card" markdown>
### `meddeid-data`

Hospital project import, stable identifiers, deterministic splits, synthetic generation, Synthea integration, and data validation.

**Install:** `pip install meddeid-data`
</div>

<div class="component-card" markdown>
### `meddeid-training`

Ordinary fitting, publication-grade epoch selection/refit, and self-contained bundle export.

**Install:** `pip install 'meddeid-training[train]'`
</div>

<div class="component-card" markdown>
### `meddeid-eval`

Exact-span and character metrics, core-PII recall, non-PII redaction, and stability analysis.

**Install:** `pip install meddeid-eval`
</div>

<div class="component-card" markdown>
### `meddeid-annotate`

Local, single-assignment primary-span annotation application. Reads and writes canonical JSONL.

**Run:** Node.js 20+ or its Dockerfile
</div>

<div class="component-card" markdown>
### `meddeid-curate`

Optional multi-annotator comparison, explicit reconciliation decisions, and audited current primary gold.

**Run:** Node.js 20+ or its Dockerfile
</div>

<div class="component-card" markdown>
### `meddeid-subannotate`

Gold-only core-PII character subannotation and checksummed benchmark-bundle export.

**Run:** Node.js 20+ or its Dockerfile
</div>

</div>

## Supporting surfaces

| Surface | Status | Responsibility |
|---|---|---|
| `meddeid.github.io` | Public | This suite documentation and landing page |
| `meddeid-demo` | Proposed | Hosted demonstration; publication is still to be decided |
| Hugging Face collection | Public artifacts | Current model, synthetic corpus, synthetic benchmark |
| Suite `publication/` | Maintainer staging | Cards, checksums, guidelines, release manifests |
| Suite `internal/` | Private maintainer records | Migration provenance and pre-publication lock |
| Suite `comparators/` | External | Independently licensed benchmark systems |

## Documentation ownership

This site explains which component to use and how cross-component workflows fit together. A component repository is authoritative for its API, CLI flags, configuration keys, tests, and release notes. Artifact cards are authoritative for model or dataset contents, limitations, licences, and immutable revisions.
