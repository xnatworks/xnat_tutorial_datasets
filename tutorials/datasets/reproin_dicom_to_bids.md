# ReproIn DICOM phantom — DICOM → BIDS conversion

## Why this dataset

The University of Arizona phantom dataset built by Yarik Halchenko's team
specifically to teach **DICOM → BIDS conversion using HeuDiConv with the
ReproIn naming convention**. Acquired on a Siemens Skyra 3T to cover the
sequences a typical neuroimaging study uses: T1w, T2w, single-band BOLD,
multi-band BOLD, single- and multi-band DWI, fieldmaps. ReproIn-style
ProtocolName fields are baked into the DICOM headers, so HeuDiConv (or
dcm2bids with the ReproIn heuristic) recognizes each series and lands it
in the right BIDS slot **without any project-specific heuristic file**.

This is the closest thing to a "standardized scanning protocol →
standardized data" demo you can do in 10 minutes.

| | |
|---|---|
| Modality | MR (multi-sequence) |
| License | HeuDiConv/ReproIn teaching materials |
| Source | [HeuDiConv ReproIn docs](https://heudiconv.readthedocs.io/en/latest/reproin.html) |
| Plugin id | `reproin_dicom_to_bids` |
| Default project | `XNAT_TUTORIAL_REPROIN` |
| Series count | 7 |
| File count | ~803 DICOM files |

## Download via the tutorial plugin

**UI** — Admin → **Tutorial Datasets** (`${XNAT_HOST}/xnat-tutorial/datasets.html`).
Find `reproin_dicom_to_bids` in the list, click **Prepare**, set the project id, submit.

**REST** (admin auth required):

```bash
# stage source files only — leaves them in the plugin staging area
curl -u ${XNAT_USER}:${XNAT_PASS} -X POST \
  ${XNAT_HOST}/xapi/tutorials/datasets/reproin_dicom_to_bids/download

# stage + create the project + import (typical)
curl -u ${XNAT_USER}:${XNAT_PASS} -X POST \
  "${XNAT_HOST}/xapi/tutorials/datasets/reproin_dicom_to_bids/prepare?projectId=XNAT_TUTORIAL_REPROIN"
```

## What you get in XNAT

- Project `XNAT_TUTORIAL_REPROIN`
- 1 subject, 1 MR session, 7 scans (T1w, T2w, BOLD ×N, DWI, fmap)
- Original DICOMs preserved on each scan

## Workshop flow

1. Inspect DICOM ProtocolName fields — note the `acq-`, `run-`, `dir-`
   conventions baked in.
2. Run a **HeuDiConv container** (or `dcm2bids` with ReproIn heuristic):
   ```bash
   curl -b "JSESSIONID=$SESS" -X POST -H 'Content-Type: application/json' \
     -d '{"session":"/archive/projects/${PROJECT}/experiments/${SESSION}"}' \
     ${XNAT_HOST}/xapi/projects/${PROJECT}/wrappers/${HEUDICONV_WRAPPER_ID}/launch
   ```
3. The output `BIDS` resource lands on the session — inspect the tree.
4. Discuss naming: `sub-<id>_ses-<id>_acq-<acq>_run-<n>_T1w.nii.gz` etc.
5. Pipe the BIDS resource into [fMRIPrep](openneuro_flanker_bids.md) or
   [RABIES](../04-rabies-rodent-fmri.md) — same input shape.

## Talking points

- ReproIn is a **scanner-side convention**: get it right at the console
  and your downstream BIDS conversion is essentially zero-config.
- Adopting ReproIn at site enrollment time is the highest-leverage
  intervention for reproducible neuroimaging data management.
- HeuDiConv vs dcm2bids: both work; dcm2bids is JSON-config, HeuDiConv
  is Python-heuristic. ReproIn ships heuristics for both.
