# NiiVue demo images — atlases and reference volumes

## Why this dataset

A curated collection of ~70 NIfTI volumes that the NiiVue project ships
to demo every viewer feature: multi-modal CT, T1/T2/PD MRI, MR
angiography, fMRI activation maps, perfusion, plus standard atlases
(MNI152, Big Brain, Allen, Cerebellum, Juelich, Thalamus, CIT168, PD25).

It's not a "dataset" in the clinical-trial sense — it's a **reference
library**. A radiologist can use it to teach windowing and contrast; a
neuroscientist can use it to demonstrate atlas-based ROI definition; a
front-end engineer can use it to test that a viewer renders weird affine
matrices, anisotropic spacing, masked overlays, and probability maps
correctly.

For an XNAT course or demo, this is the fastest way to populate a project with
visually distinct, label-rich volumes you can use to demo the viewer
stack (Workbench / NiiVue / OHIF / VolView).

| | |
|---|---|
| Modality | CT, MR (T1/T2/PD/FLAIR/MRA), fMRI, atlases |
| Format | NIfTI (single-file `.nii.gz`) |
| License | BSD-2-Clause repo + per-image upstream notes |
| Source | [niivue/niivue-demo-images](https://github.com/niivue/niivue-demo-images) |
| Plugin id | `niivue_demo_images` |
| Default project | `XNAT_TUTORIAL_NIIVUE` |
| Volumes | 70+ NIfTI files |

## Download via the tutorial plugin

**UI** — Admin → **Tutorial Datasets** (`${XNAT_HOST}/xnat-tutorial/datasets.html`).
Find `niivue_demo_images` in the list, click **Prepare**, set the project id, submit.

**REST** (admin auth required):

```bash
# stage source files only — leaves them in the plugin staging area
curl -u ${XNAT_USER}:${XNAT_PASS} -X POST \
  ${XNAT_HOST}/xapi/tutorials/datasets/niivue_demo_images/download

# stage + create the project + import (typical)
curl -u ${XNAT_USER}:${XNAT_PASS} -X POST \
  "${XNAT_HOST}/xapi/tutorials/datasets/niivue_demo_images/prepare?projectId=XNAT_TUTORIAL_NIIVUE"
```

## What you get in XNAT

- Project `XNAT_TUTORIAL_NIIVUE`
- A project-level `NIFTI` resource containing the volumes (no per-scan
  ingest — these are reference images, not patient sessions)

## Beginner checkpoints

What this dataset teaches: not every useful XNAT file collection needs to be a
DICOM session. Project-level resources are useful for shared reference images,
atlases, and viewer test files.

What to look for in XNAT: open the project resources and inspect the `NIFTI`
resource file list.

How to know import worked: the project-level resource contains many visually
distinct `*.nii` or `*.nii.gz` files, including templates, atlases, and example
contrast images.

What to check first if it does not: make sure you are looking at project
resources rather than a session scan table. This dataset is loaded as files,
not as DICOM scans.

## Walkthrough

Pick 2-3 contrasting volumes and walk through:

1. **CT_Abdo.nii.gz** — abdominal CT, demo windowing (lung/soft tissue/bone)
2. **chris_t1.nii.gz** + **chris_t2.nii.gz** — same subject, two contrasts; demo NiiVue's multi-volume blending
3. **AllenAtlas.nii.gz** + **ICBM2009sym.nii.gz** — atlas + template; demo label-overlay opacity, NiiVue colormap legends
4. **fmri_pitch.nii.gz** — 4D activation; demo time-series scrubbing
5. **CIT168/CIT168.nii.gz** — subcortical probabilistic atlas; demo
   atlas-driven ROI extraction

## Talking points

- NIfTI vs DICOM at a glance: single file vs hundreds, no series
  metadata vs rich metadata. Most research workflows operate in NIfTI.
- Atlases are first-class citizens in research projects — XNAT's
  project-level resource lets you ship them once and reference them
  from every session.
- Pair with [Workbench plugin](https://github.com/xnatworks/xnat_workbench)
  / OHIF for an end-to-end "load → view → annotate → save" demo.
