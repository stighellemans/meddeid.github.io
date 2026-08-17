# MedDeID documentation

This repository builds the public MedDeID documentation portal. It owns
cross-suite navigation, user journeys, architecture, shared concepts, and the
component map. Component repositories remain authoritative for their own API,
CLI, configuration, implementation, and release notes.

## Documentation structure

- `docs/start/`: goal-oriented entry points and installation.
- `docs/workflows/`: tasks that cross one or more suite components.
- `docs/concepts/`: architecture, the canonical data contract, and lineage.
- `docs/reference/`: component ownership and release compatibility.
- `docs/artifacts/`: public model, dataset, guideline, and demo orientation.
- `docs/project/`: privacy, contribution, licence, and citation guidance.

Artifact-specific contents, limitations, licences, hashes, and revisions belong
in their Hugging Face or archival cards. Suite migration and release-staging
records belong in the coordinator workspace, not on the public site.

## Preview locally

```bash
python -m venv .venv
. .venv/bin/activate
pip install -r requirements-docs.txt
python scripts/check_docs.py
mkdocs serve
```

Build the exact static site with:

```bash
mkdocs build --strict
```

GitHub Actions checks every pull request. A push to `main` builds and deploys
the reviewed site to GitHub Pages.

## Editing policy

Before adding a page, identify its authoritative owner:

| Subject | Owner |
|---|---|
| Cross-suite choice, workflow, or architecture | This repository |
| Package API, CLI, configuration, or behavior | Component repository |
| Canonical schema and taxonomy | `meddeid-core` |
| Language-profile behavior and locale resources | Relevant `meddeid-language-*` repository |
| Model or dataset facts | Artifact card |
| Migration, release locks, or private provenance | Suite coordinator |

Link to authoritative detail instead of copying it. Update
`docs/reference/compatibility.md` with every coordinated release.

## Licence

See `LICENSE` and `NOTICE`.
