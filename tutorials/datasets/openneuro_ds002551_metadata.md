# OpenNeuro ds002551 rodent participant metadata

## Why this dataset

This is the non-imaging metadata slice from OpenNeuro `ds002551`,
`Mouse_rest_awake`. It contains a tiny `participants.tsv` table plus the BIDS
data dictionary that explains the values. The files are small, but they are
ideal for teaching why XNAT sometimes needs project-specific structured
metadata in addition to images.

Use this dataset with [Tutorial 03 — Preclinical Metadata as a Dynamic Data
Type](../03-preclinical-metadata-datatype.md). The learner can read the table,
create a dynamic subject-assessor datatype, add a custom form, and enter rows
as searchable XNAT records.

| | |
|---|---|
| Modality | tabular metadata |
| Format | TSV + JSON data dictionary |
| License | CC0 |
| Source | [OpenNeuro ds002551](https://openneuro.org/datasets/ds002551) |
| Plugin id | `openneuro_ds002551_metadata` |
| Default project | `XNAT_TUTORIAL_RODENT` |
| Files | `dataset_description.json`, `participants.tsv`, `participants.json` |

## Download via the tutorial plugin

**UI** — Admin -> **Tutorial Datasets** (`${XNAT_HOST}/xnat-tutorial/datasets.html`).
Find `openneuro_ds002551_metadata` in the list, click **Prepare**, set the
project id, submit.

**REST** (admin auth required):

```bash
# stage source files only
curl -u ${XNAT_USER}:${XNAT_PASS} -X POST \
  ${XNAT_HOST}/xapi/tutorials/datasets/openneuro_ds002551_metadata/download

# stage + create the project + import
curl -u ${XNAT_USER}:${XNAT_PASS} -X POST \
  "${XNAT_HOST}/xapi/tutorials/datasets/openneuro_ds002551_metadata/prepare?projectId=XNAT_TUTORIAL_RODENT"
```

## What you get in XNAT

- Project `XNAT_TUTORIAL_RODENT`
- Project-level `TUTORIAL_METADATA` resource
- `participants.tsv`, `participants.json`, and `dataset_description.json`
- No imaging sessions or scans from this metadata-only subset

## Beginner checkpoints

What this dataset teaches: study-specific metadata can be reviewed and turned
into searchable XNAT records without writing a custom plugin.

What to look for in XNAT: open project resources, then open
`TUTORIAL_METADATA` and inspect `participants.tsv` beside `participants.json`.

How to know import worked: the project resource contains all three metadata
files and `participants.tsv` has seven mouse participant rows.

What to check first if it does not: make sure you are looking at project
resources, not the scan table. This dataset intentionally has no image sessions
or DICOM scans.

## Walkthrough

1. Open `participants.tsv` and identify the columns: participant ID, sex,
   field strength, coil, breathing condition, and sedation.
2. Open `participants.json` and read the meaning of the coded values.
3. Decide which fields a curator would want to search later.
4. Follow [Tutorial 03](../03-preclinical-metadata-datatype.md) to create a
   `Rodent Acquisition Metadata` subject assessor and custom form.
5. Enter one participant row as a form-backed XNAT record.
6. Search or filter for a value such as `Awake` sedation or `Bruker Cryoprobe`
   coil.

## Talking points

- BIDS data dictionaries are good starting points for XNAT custom forms.
- The source data is tiny, so this is a reliable live admin tutorial.
- The same pattern works for cohort labels, behavioral scores, acquisition
  settings, and manual QC fields.
