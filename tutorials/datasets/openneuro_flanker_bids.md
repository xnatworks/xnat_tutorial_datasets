# `openneuro_flanker_bids` — OpenNeuro ds000102 Flanker

A single subject from one of the earliest publicly shared task-fMRI
datasets — the Eriksen Flanker conflict-monitoring paradigm. Already in
BIDS layout with anatomical T1w, BOLD, and `events.tsv`. Small enough to
land + preprocess + analyse in one workshop slot.

| | |
|---|---|
| Modality | MR (T1w + BOLD) |
| Task | Eriksen Flanker |
| Format | BIDS / NIfTI |
| License | PDDL (older OpenNeuro convention; equivalent to public domain) |
| Source | [OpenNeuro ds000102](https://openneuro.org/datasets/ds000102) |
| Plugin id | `openneuro_flanker_bids` |
| Default project | `XNAT_TUTORIAL_BIDS` |
| Subjects | 1 (`sub-01`) |

## Load it

UI — **Tools → Tutorial Datasets**, find `openneuro_flanker_bids`,
**Prepare**.

REST — see
[reference/rest-cheatsheet § Tutorial dataset loader](../reference/rest-cheatsheet.md#tutorial-dataset-loader).

## What you get in XNAT

- Project `XNAT_TUTORIAL_BIDS`.
- 1 subject (`sub-01`) with anatomical and functional data.
- Session-level `BIDS` resource — already in BIDS layout, no conversion
  needed.

## Lessons that use this dataset

- Intro: [06 resource browser](../intro/06-resource-browser.md),
  [07 search and export](../intro/07-search-and-export.md),
  [08 BIDS as a resource](../intro/08-bids-as-resource.md).
- Intermediate: [03 BIDS validation](../intermediate/03-bids-validation.md),
  [05 container basics](../intermediate/05-container-basics.md),
  [06 MRIQC assessor](../intermediate/06-mriqc-assessor.md).
- Advanced: serves as the BIDS-input fallback for
  [01 complete BIDS](../advanced/01-complete-bids.md) when starting from
  raw DICOM is not appropriate.
