# `niivue_demo_images` — atlases and reference volumes

A curated collection of ~70 NIfTI volumes that the NiiVue project ships
to demo every viewer feature: multi-modal CT, T1/T2/PD MRI, MR
angiography, fMRI activation, plus standard atlases (MNI152, BigBrain,
Allen, Cerebellum, Juelich, Thalamus, CIT168, PD25). It is a **reference
library**, not a clinical dataset.

For a workshop, this is the fastest way to populate a project with
visually distinct, label-rich volumes for demoing the viewer stack.

| | |
|---|---|
| Modality | CT, MR (T1/T2/PD/FLAIR/MRA), fMRI, atlases |
| Format | NIfTI (single-file `.nii.gz`) |
| License | BSD-2-Clause repo + per-image upstream notes (some CC BY-NC 4.0) |
| Source | [niivue/niivue-demo-images](https://github.com/niivue/niivue-demo-images) |
| Plugin id | `niivue_demo_images` |
| Default project | `XNAT_TUTORIAL_NIIVUE` |
| Volumes | 70+ NIfTI files, ~145 MB total |

## Load it

UI — **Tools → Tutorial Datasets**, find `niivue_demo_images`,
**Prepare**.

REST — see
[reference/rest-cheatsheet § Tutorial dataset loader](../reference/rest-cheatsheet.md#tutorial-dataset-loader).

## What you get in XNAT

- Project `XNAT_TUTORIAL_NIIVUE`.
- A project-level `NIFTI` resource with the volumes (no per-scan ingest
  — these are reference images, not patient sessions).

## Lessons that use this dataset

- Intro: [09 NiiVue overlays](../intro/09-niivue-overlays.md).
- Intermediate: alternate dataset for
  [09 preclinical MRI](../intermediate/09-preclinical-mri.md) when
  contrasting human and rodent voxel scales.
