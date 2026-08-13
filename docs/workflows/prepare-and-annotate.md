# Prepare and annotate data

This workflow turns hospital exports into stable canonical records and then into reviewed primary PII annotations.

## 1. Create a local project

<span class="source-label">Owner: meddeid-data</span>

```bash
pip install 'meddeid-data[parquet]'

meddeid-data project create my-project notes.parquet \
  --namespace hospital-study \
  --language-profile nl-BE \
  --id-column note_id \
  --text-column note_text
```

The same command accepts CSV, TSV, Parquet, or a directory of UTF-8 `.txt` files. Table columns other than the ID and text columns become metadata by default.

The project writes pseudonymous stable document IDs into canonical artifacts. Its HMAC key and reversible source-ID mapping remain under the gitignored `private/` directory.

!!! danger "Keep the private directory private"
    Losing the HMAC key breaks stable re-import identity. Publishing the source-ID map defeats pseudonymization. Back it up and protect it according to institutional policy.

## 2. Create model-initialized assignments

```bash
meddeid batch my-project/artifacts/annotations.jsonl \
  --output my-project/assignments/primary.jsonl
```

The predictions are ordinary current spans in an unreviewed assignment, not a second suggestion format. A reviewer keeps, corrects, relabels, deletes, or adds spans and then marks the document reviewed.

For fully blind annotation, begin with the empty imported records instead.

## 3. Review primary spans

<span class="source-label">Owner: meddeid-annotate</span>

```bash
npm install --prefix /path/to/meddeid-annotate

MEDDEID_ANNOTATIONS_PATH="$PWD/my-project/assignments/primary.jsonl" \
npm --prefix /path/to/meddeid-annotate run dev
```

The application writes the configured canonical JSONL in place. For every document, inspect the complete text—not just existing spans—and save even a reviewed document with zero spans.

Never point two reviewers at the same writable assignment. Give each reviewer an isolated copy.

## 4. Package a completed annotation set

```bash
meddeid-data project package-annotation my-project \
  my-project/assignments/reviewer-a.jsonl \
  --annotation-set-id hospital-study-round-1 \
  --annotator-id reviewer-7
```

The manifest gives the file durable identity and pins its checksum and contract versions. A bare JSONL file remains usable, but a manifested set is preferable for curation and reproducibility.

## 5. Curate only when required

<span class="source-label">Owner: meddeid-curate</span>

Use `meddeid-curate` when two or more independent annotation sets must be reconciled. It retains exact agreements, groups disagreements, records explicit decisions, and blocks publication until every disagreement and whole document is confirmed.

```text
completed reviewer A + completed reviewer B
  → meddeid-curate
  → annotations.jsonl + decisions.jsonl + manifest.json
```

One completed reviewer can skip this step. Curation is a study-design decision, not a technical requirement imposed by MedDeID.

## 6. Add core-PII subannotations only for evaluation

<span class="source-label">Owner: meddeid-subannotate</span>

`meddeid-subannotate` accepts completed primary gold from either `meddeid-annotate` or `meddeid-curate`. It partitions each primary span into reviewed character segments and exports a checksummed evaluation bundle.

Do not use it for training data or prediction review. Its purpose is to define the core-PII recall denominator for detailed benchmark evaluation.

## Outputs and owners

| Artifact | Produced by | Consumed by |
|---|---|---|
| Imported canonical JSONL | `meddeid-data` | `meddeid`, `meddeid-annotate` |
| Completed annotation set | `meddeid-annotate` | `meddeid-data`, optional `meddeid-curate`, training |
| Curated primary gold | `meddeid-curate` | `meddeid-subannotate`, evaluation |
| Evaluation bundle | `meddeid-subannotate` | `meddeid-eval` |

See [artifact lineage](../concepts/artifact-lineage.md) for the identity and checksum rules at each handoff.
