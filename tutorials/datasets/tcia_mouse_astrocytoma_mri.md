# `tcia_mouse_astrocytoma_mri` — preclinical mouse MRI

Anatomical MRI from the Mouse-Astrocytoma collection at TCIA. The
teaching value is two-fold: (1) the same archive, viewer, and pipeline
tooling works unchanged on a mouse brain MRI; (2) side-by-side with a
human MR, the physical-scale differences motivate why preclinical
pipelines (RABIES, AIDAmri) need different atlases and motion priors.

| | |
|---|---|
| Subject | mouse (orthotopic glioma model) |
| Modality | MR (T2w in this slice) |
| Format | DICOM |
| License | CC-BY 3.0 |
| Source | [TCIA Mouse-Astrocytoma](https://www.cancerimagingarchive.net/collection/mouse-astrocytoma/) |
| Plugin id | `tcia_mouse_astrocytoma_mri` |
| Default project | `XNAT_TUTORIAL_MOUSE` |

## Load it

UI — **Tools → Tutorial Datasets**, find `tcia_mouse_astrocytoma_mri`,
**Prepare**.

REST — see
[reference/rest-cheatsheet § Tutorial dataset loader](../reference/rest-cheatsheet.md#tutorial-dataset-loader).

## What you get in XNAT

- Project `XNAT_TUTORIAL_MOUSE`.
- 1 subject (mouse), 1 MR session, 1 anatomical scan with native DICOM.

## Lessons that use this dataset

- Intro: usable as the alternate dataset for
  [02 XNAT hierarchy](../intro/02-xnat-hierarchy.md) and
  [05 viewer basics](../intro/05-viewer-basics.md).
- Intermediate: [01 dcm2niix](../intermediate/01-dcm2niix.md) (rodent
  data converts unchanged), [09 preclinical MRI](../intermediate/09-preclinical-mri.md).
- Pair with `openneuro_ds002551_metadata` to bring in matching cohort
  metadata.
