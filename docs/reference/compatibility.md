# Compatibility

Compatibility is defined by declared package ranges and machine-readable contracts, not by sibling checkout proximity.

## Current compatibility line

| Component | Version | Distribution | Direct MedDeID contract dependencies |
|---|---:|---|---|
| `meddeid-core` | 0.1.0 | PyPI | None |
| `meddeid-language-nl` | 0.1.0 | PyPI | `meddeid-core >=0.1,<0.2` |
| `meddeid` | 0.1.0 | PyPI and GHCR API image | `meddeid-core >=0.1,<0.2`; `meddeid-language-nl >=0.1,<0.2` |
| `meddeid-data` | 0.2.0 | PyPI | `meddeid-core >=0.1,<0.2`; `meddeid-language-nl >=0.1,<0.2` |
| `meddeid-eval` | 0.2.0 | PyPI | `meddeid-core >=0.1`; `meddeid-language-nl >=0.1` |
| `meddeid-training` | 0.1.0 | PyPI | `meddeid-core >=0.1,<0.2`; `meddeid-eval >=0.2,<0.3` |
| Browser applications | 0.1.0 | Public source/Dockerfile | Generated taxonomy contract version 1 |

All Python packages require Python 3.10 or later. The annotation applications require Node.js 20 or later for source-based local use.

!!! note "Published and source-only components"
    All six Python packages are public releases. Browser-application versions
    identify the reviewed public source line and do not imply an npm release.
    Published package metadata and immutable artifact manifests take
    precedence. Update this page as part of every coordinated release.

## Shared contracts

| Contract | Current value | Authority |
|---|---|---|
| Record schema | `meddeid.schema.v1` | `meddeid-core` |
| Offset unit | `unicode_codepoints` | `meddeid-core` |
| Taxonomy contract | version 1 | `meddeid-core/contracts/taxonomy.json` |
| Taxonomy | `ProductionLabels-v1.1` | Core taxonomy plus published annotation guidelines |
| Current public language profile | `nl-BE@1` | `meddeid-language-nl` |
| Subannotation profile contract | `meddeid.subannotation-profile.v1` | `meddeid-subannotate` |
| Persisted profile selection | `meddeid.subannotation-profile-selection.v1` | `meddeid-subannotate` |
| Built-in subannotation profile | `neutral@1` / `core-pii-neutral@1` | `meddeid-subannotate` |
| Belgian-Dutch subannotation profile | `nl-BE@1` / `core-pii-nl-be@1` | `meddeid-language-nl` |
| Default model | `stighellemans/meddeid-dutch-synth` | Published model bundle |

## Release rule

A coordinated release should pin:

1. the immutable Git revision of every component;
2. built wheel or application checksums;
3. schema, taxonomy, and language-profile identities;
4. immutable model and dataset revisions;
5. documentation revision;
6. end-to-end verification results.

Patch releases may clarify documentation or fix behavior without changing a contract. Any incompatible record, taxonomy, profile, or bundle change requires an explicit new contract/version and migration guidance.
