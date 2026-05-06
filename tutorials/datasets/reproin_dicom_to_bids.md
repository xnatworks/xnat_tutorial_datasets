# `reproin_dicom_to_bids` — ReproIn DICOM phantom

The University of Arizona phantom dataset built specifically to teach
DICOM → BIDS conversion using HeuDiConv with the **ReproIn** naming
convention. ReproIn-style `ProtocolName` fields are baked into the DICOM
headers, so the conversion lands each series in the right BIDS slot
**without a project-specific heuristic file**.

| | |
|---|---|
| Modality | MR (T1w, T2w, BOLD, multi-band BOLD, DWI, fieldmaps) |
| Format | DICOM with ReproIn-encoded `ProtocolName` |
| License | HeuDiConv / ReproIn teaching materials |
| Source | [HeuDiConv ReproIn docs](https://heudiconv.readthedocs.io/en/latest/reproin.html) |
| Plugin id | `reproin_dicom_to_bids` |
| Default project | `XNAT_TUTORIAL_REPROIN` |
| Series | 7 |
| Files | ~803 DICOM files |

## Load it

UI — **Tools → Tutorial Datasets**, find `reproin_dicom_to_bids`,
**Prepare**.

REST — see
[reference/rest-cheatsheet § Tutorial dataset loader](../reference/rest-cheatsheet.md#tutorial-dataset-loader).

## What you get in XNAT

- Project `XNAT_TUTORIAL_REPROIN`.
- 1 subject, 1 MR session, 7 scans (T1w, T2w, BOLD ×N, DWI, fmap).
- Original DICOMs preserved on each scan.

## Lessons that use this dataset

- Intermediate: [04 DICOM to BIDS](../intermediate/04-dicom-to-bids.md)
  (alternate dataset to `bidscoin_dicom_to_bids`).
- Advanced: [01 complete BIDS](../advanced/01-complete-bids.md)
  (alternate DICOM source). Note: if `dcm2bids-session` fails on scan
  `16`, see the troubleshooting section of that lesson — read stderr
  before assuming the upstream archive is broken.
