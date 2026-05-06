# `msd_hippocampus_nnunet` — MSD Task04 Hippocampus

The de-facto first-time-with-nnU-Net dataset: 263 mono-modal MR volumes
(T1w, hippocampus-cropped) with hand-drawn anterior + posterior
hippocampus labels. Small enough to train a 3d_fullres nnU-Net end-to-
end on a single GPU in under an hour.

The XNAT loader maps each MSD case to the standard project / subject /
session hierarchy with both the source NIfTI and the label NIfTI as
distinct resources, so a learner can inspect training data, run
inference, and compare predictions to ground truth.

| | |
|---|---|
| Modality | MR (T1w) |
| Task | 3-class segmentation: background / anterior hippocampus / posterior hippocampus |
| Format | NIfTI source + label volumes |
| License | CC-BY-SA 4.0 |
| Source | [Medical Segmentation Decathlon](https://decathlon-10.grand-challenge.org/) |
| Plugin id | `msd_hippocampus_nnunet` |
| Loader | `grouplevel_nnunet_msd` |
| Default project | `XNAT_TUTORIAL_SEG` |
| Tutorial subset | 10 training cases (full set on request) |

## Load it

UI — **Tools → Group-Level Datasets** (or the Group-Level entry in
**Tools → XNAT Tutorial & Walkthrough**), find
`msd_hippocampus_nnunet`, **Prepare**.

REST — see
[reference/rest-cheatsheet § Tutorial dataset loader](../reference/rest-cheatsheet.md#tutorial-dataset-loader).

## What you get in XNAT

- Project `XNAT_TUTORIAL_SEG`.
- 10 subjects, 1 session each, T1w scan + label NIfTI per session.
- Provenance recorded at project level (source URL, MSD task ID,
  license).

## Lessons that use this dataset

- Advanced: [02 nnU-Net dataset](../advanced/02-nnunet-dataset.md).
