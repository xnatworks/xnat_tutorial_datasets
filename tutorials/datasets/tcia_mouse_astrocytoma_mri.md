# TCIA Mouse-Astrocytoma — preclinical mouse MRI

## Why this dataset

A small slice of the **Mouse-Astrocytoma** collection at TCIA: anatomical
MRI of glioblastoma-bearing mice acquired during a longitudinal study of
tumor growth and treatment response. The full collection is multi-time-
point, multi-modal (T2w anatomy, contrast-enhanced T1w, DWI, sometimes
DCE), and ships with hand-drawn tumor segmentations on a subset of
sessions.

The teaching value is two-fold:

1. **Non-human imaging in DICOM**. Most XNAT demos use human data;
   showing that the same archive, viewer, and pipeline tooling works
   unchanged on a mouse brain MRI is the cleanest "preclinical isn't
   special" argument for an audience used to clinical workflows.
2. **Comparative anatomy**. Side-by-side with a human MR (e.g. from
   `tcia_dicom_intro` or `niivue_demo_images`) you can teach the
   physical-scale differences that drive why preclinical pipelines like
   RABIES use different atlases, voxel sizes, and motion priors than
   fMRIPrep.

| | |
|---|---|
| Subject | mouse (orthotopic glioma model) |
| Modality | MR (T2w shown here) |
| License | CC-BY 3.0 |
| Source | [TCIA Mouse-Astrocytoma](https://www.cancerimagingarchive.net/collection/mouse-astrocytoma/) |
| Plugin id | `tcia_mouse_astrocytoma_mri` |
| Default project | `XNAT_TUTORIAL_MOUSE` |

## Download via the tutorial plugin

**UI** — Admin → **Tutorial Datasets** (`${XNAT_HOST}/xnat-tutorial/datasets.html`).
Find `tcia_mouse_astrocytoma_mri` in the list, click **Prepare**, set the project id, submit.

**REST** (admin auth required):

```bash
# stage source files only — leaves them in the plugin staging area
curl -u ${XNAT_USER}:${XNAT_PASS} -X POST \
  ${XNAT_HOST}/xapi/tutorials/datasets/tcia_mouse_astrocytoma_mri/download

# stage + create the project + import (typical)
curl -u ${XNAT_USER}:${XNAT_PASS} -X POST \
  "${XNAT_HOST}/xapi/tutorials/datasets/tcia_mouse_astrocytoma_mri/prepare?projectId=XNAT_TUTORIAL_MOUSE"
```

## What you get in XNAT

- Project `XNAT_TUTORIAL_MOUSE`
- 1 subject (mouse), 1 MR session, 1 anatomical scan with native DICOM

## Beginner checkpoints

What this dataset teaches: mouse MRI still fits the same XNAT hierarchy, but
the metadata and downstream analysis choices differ from human imaging.

What to look for in XNAT: open the session scan table, scan metadata, DICOM
headers, and viewer. Compare the image scale and voxel size to a human MR
example.

How to know import worked: the project has one mouse MR session with an
anatomical scan and native DICOM files.

What to check first if it does not: confirm the TCIA download completed and
that the session is archived. If the viewer looks unusual, verify voxel size
and orientation before assuming import failed.

## What to do with it

| Goal | Tool |
|---|---|
| DICOM → NIfTI | [`dcm2niix`](../01-dcm2niix.md) — works on rodent data unchanged |
| Brain extraction | rodent-specific atlas-based methods (PCNN3D, RATS); SynthStrip works surprisingly well too |
| Atlas registration | Allen Mouse Brain Atlas — see `AIDAmri` |
| Full preclinical pipeline | [RABIES](../07-rabies-rodent-fmri.md) (needs a paired functional series — pair with OpenNeuro ds002551 for that) |

## Walkthrough

1. Open the session in the XNAT viewer. Note voxel size (typically
   ~100 µm isotropic for mouse MRI vs 1 mm for human).
2. Run dcm2niix → inspect the resulting NIfTI in NiiVue alongside a
   human T1 from `niivue_demo_images`. Same viewer, two scales.
3. Discuss preclinical-specific considerations:
   - Coil geometry → different bias-field correction
   - No skull, but lots of muscle/fat → SynthStrip or rodent-specific
     extractors needed
   - Atlas: Allen Mouse Brain CCFv3 or Australian Mouse Brain Mapping
     Consortium (AMBMC)
4. (Optional) Pull in OpenNeuro ds002551 functional data and run RABIES.

## Talking points

- TCIA hosts a surprising amount of preclinical data — Mouse-
  Astrocytoma, Mouse-Mammary, RIDER-Lung-PET-CT-Mouse, more. XNAT
  ingests them with no special handling.
- any XNAT tutorial program (the program adjacent to this tutorial) explicitly
  bridges preclinical and clinical data management — this dataset is
  the symbol of that bridge.
- The XNAT data model has no concept of "patient species". Custom
  variables on subjects let you tag species, strain, sex, age-at-scan,
  treatment group — same archive serves humans and mice.
