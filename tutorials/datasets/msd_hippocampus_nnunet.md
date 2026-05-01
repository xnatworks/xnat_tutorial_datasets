# MSD Task04 Hippocampus — nnU-Net training & inference

## Why this dataset

The Hippocampus task from the **Medical Segmentation Decathlon (MSD)**
is the de facto first-time-with-nnU-Net dataset. 263 mono-modal MR
volumes (T1w, hippocampus-cropped) with hand-drawn anterior + posterior
hippocampus labels — small enough to train an nnU-Net 3d_fullres model
end-to-end on a single GPU in under an hour, complex enough that the
result is a non-trivial 3-class segmentation.

It's the dataset behind Isensee et al.'s nnU-Net paper and the
standard "did my install work?" benchmark for the framework. Loading it
into XNAT lets a tutorial demo the **full lifecycle**: data → train →
inference → segmentation back into the project archive.

| | |
|---|---|
| Modality | MR (T1w) |
| Task | 3-class segmentation: background, anterior hippocampus, posterior hippocampus |
| License | CC-BY-SA 4.0 |
| Source | [Medical Segmentation Decathlon](https://decathlon-10.grand-challenge.org/) |
| Plugin id | `msd_hippocampus_nnunet` |
| Loader | `grouplevel_nnunet_msd` |
| Default project | `XNAT_TUTORIAL_SEG` |
| Default training cases | 10 (tutorial subset) |

## Download via the tutorial plugin

**UI** — Admin → **Tutorial Datasets** (`${XNAT_HOST}/xnat-tutorial/datasets.html`).
Find `msd_hippocampus_nnunet` in the list, click **Prepare**, set the project id, submit.

**REST** (admin auth required):

```bash
# stage source files only — leaves them in the plugin staging area
curl -u ${XNAT_USER}:${XNAT_PASS} -X POST \
  ${XNAT_HOST}/xapi/tutorials/datasets/msd_hippocampus_nnunet/download

# stage + create the project + import (typical)
curl -u ${XNAT_USER}:${XNAT_PASS} -X POST \
  "${XNAT_HOST}/xapi/tutorials/datasets/msd_hippocampus_nnunet/prepare?projectId=XNAT_TUTORIAL_SEG"
```

## What you get in XNAT

- Project `XNAT_TUTORIAL_SEG`
- 10 subjects, 1 session each, T1w scan + label NIfTI per session
- The label resource carries the ground-truth segmentation (the part
  that distinguishes a *training* dataset from a *test* one)

## Walkthrough

1. Inspect a session: T1w image + matching label NIfTI overlaid in
   Workbench.
2. **Train** (optional, slow):
   ```bash
   nnUNetv2_train Task04_Hippocampus 3d_fullres 0
   ```
   Discuss why an out-of-the-box convnet works on this dataset (heavy
   spatial priors, mono-modal, small).
3. **Run inference** with the wrapped `nnunet` container against a held-
   out subject. Output is a multi-label NIfTI written back as a
   `nnUNet_SEG` resource.
4. Compare predicted vs ground-truth using Dice — XNAT can launch a
   scoring container, or do it inline:
   ```python
   import nibabel as nib, numpy as np
   gt = nib.load("label.nii.gz").get_fdata().astype(int)
   pr = nib.load("prediction.nii.gz").get_fdata().astype(int)
   for l in [1, 2]:
       d = 2 * ((gt==l) & (pr==l)).sum() / ((gt==l).sum() + (pr==l).sum())
       print(f"label {l} Dice: {d:.3f}")
   ```

## Talking points

- nnU-Net is the strongest baseline for medical-image segmentation —
  beating it requires dataset-specific architectural priors. Most
  papers reporting "novel SOTA" actually compare to nnU-Net 3d_fullres.
- The MSD "Task04" naming convention is universal in segmentation
  literature; sharing this dataset structure keeps your XNAT
  consistent with public benchmarks.
- For sites running their own segmentation studies, the same Group-
  Level loader pattern handles arbitrary "subject + image + label" data.
