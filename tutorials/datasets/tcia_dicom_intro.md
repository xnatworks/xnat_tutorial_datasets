# TCIA QIN-PROSTATE-Repeatability — DICOM intro

## Why this dataset

A single MR series, 14 slices, ~700 KB zipped — the smallest possible
"real DICOM" you can put through XNAT. Pulled from the **Quantitative
Imaging Network (QIN) Prostate Repeatability** collection at the National
Cancer Institute's Cancer Imaging Archive (TCIA), where it lives
alongside 15 patients × 2 visits of T2 + DWI prostate MRI used to
benchmark scanner-to-scanner reliability of quantitative biomarkers.

This series is a slice of a clinically meaningful dataset, but tiny
enough to demo end-to-end in a few seconds.

| | |
|---|---|
| Modality | MR |
| Body part | Prostate |
| License | CC-BY-4.0 |
| Source | [TCIA QIN-PROSTATE-Repeatability](https://www.cancerimagingarchive.net/collection/qin-prostate-repeatability/) |
| Plugin id | `tcia_dicom_intro` |
| Default project | `XNAT_TUTORIAL_DICOM` |

## Download via the tutorial plugin

**UI** — Admin → **Tutorial Datasets** (`${XNAT_HOST}/xnat-tutorial/datasets.html`).
Find `tcia_dicom_intro` in the list, click **Prepare**, set the project id, submit.

**REST** (admin auth required):

```bash
# stage source files only — leaves them in the plugin staging area
curl -u ${XNAT_USER}:${XNAT_PASS} -X POST \
  ${XNAT_HOST}/xapi/tutorials/datasets/tcia_dicom_intro/download

# stage + create the project + import (typical)
curl -u ${XNAT_USER}:${XNAT_PASS} -X POST \
  "${XNAT_HOST}/xapi/tutorials/datasets/tcia_dicom_intro/prepare?projectId=XNAT_TUTORIAL_DICOM"
```

## What you get in XNAT

- 1 project (created if missing)
- 1 subject, 1 session, 1 MR scan
- DICOM resource on the scan (14 files)

## What to do with it

| Goal | Container / step |
|---|---|
| Convert DICOM → NIfTI | [`dcm2niix`](../01-dcm2niix.md) |
| Inspect headers | XNAT scan details page → **DICOM Headers** |
| View | OHIF Viewer / VolView / Workbench |

After dcm2niix, the NIfTI is small enough to download and open in any
local viewer. Pair with `tcia_pseudo_phi_deid` to compare anonymization
state of headers.

## Talking points

- TCIA exposes an NBIA REST API; the plugin pulls a single series UID via
  `getImage?SeriesInstanceUID=…`, so the same workflow scales from this
  one tiny series to entire multi-thousand-patient collections.
- The dataset is the source of figures in:
  Fedorov et al., *Computing the volume of the prostate using…*, Magn
  Reson Imaging 2018 — [doi:10.7937/K9/TCIA.2018.MR1CKGND](https://doi.org/10.7937/K9/TCIA.2018.MR1CKGND).
