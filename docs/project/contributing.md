# Contributing to documentation

The documentation follows a single-source ownership model. Improve information at its authoritative home, then link to it elsewhere.

## Where a change belongs

| Change | Authoritative location |
|---|---|
| Choosing components or a cross-suite workflow | This documentation site |
| Package API, CLI flag, configuration, or internal behavior | That component repository |
| Canonical record or taxonomy rule | `meddeid-core` |
| Dutch profile or lookup-resource behavior | `meddeid-language-nl` |
| Model or dataset limitations, contents, hashes, licence | Its artifact card |
| Release staging, locks, migration provenance | Suite coordinator workspace |
| Historical UX evidence | Suite pilot report |

## Page types

Use four recognizable forms:

- **Tutorial:** a complete learning journey with a defined outcome.
- **How-to:** steps for one real task.
- **Explanation:** concepts, architecture, and design rationale.
- **Reference:** exact contracts, commands, and compatibility facts.

Do not mix a release checklist into a user tutorial or a historical audit into an API reference.

## Writing rules

- Start from the user outcome.
- Name the owning component early.
- Use canonical field and command names exactly.
- State privacy boundaries next to the action that crosses them.
- Prefer one tested command over several equivalent variants.
- Link to authoritative detail instead of copying it.
- Label pilot-scale examples as interface tests, not performance evidence.
- Never include real patient or caregiver information.

## Build locally

```bash
python -m venv .venv
. .venv/bin/activate
pip install -r requirements-docs.txt
python scripts/check_docs.py
mkdocs serve
```

The continuous-integration build runs the same structural check and a strict MkDocs build. A documentation change is complete only when its local links resolve and its owning source remains clear.

## Release maintenance

For each coordinated release:

1. update the compatibility page from released metadata;
2. verify examples against released commands;
3. review privacy and limitation language;
4. check every public artifact link and immutable revision;
5. publish the docs from the same reviewed release lock.
