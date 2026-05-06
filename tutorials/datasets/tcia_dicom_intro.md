# `tcia_dicom_intro` — TCIA QIN-PROSTATE-Repeatability MR

A single MR series (~14 slices, ~700 KB zipped) — the smallest "real
DICOM" you can put through XNAT. Drawn from the TCIA QIN-PROSTATE-
Repeatability collection.

| | |
|---|---|
| Modality | MR (prostate) |
| Format | DICOM |
| License | CC-BY 4.0 |
| Source | [TCIA QIN-PROSTATE-Repeatability](https://www.cancerimagingarchive.net/collection/qin-prostate-repeatability/) |
| Plugin id | `tcia_dicom_intro` |
| Default project | `XNAT_TUTORIAL_DICOM` |
| Series UID | `1.3.6.1.4.1.14519.5.2.1.3671.4754.216645718571142540899096273555` |

## Load it

UI — **Tools → Tutorial Datasets**, find `tcia_dicom_intro`, **Prepare**,
keep the default project ID.

REST — see
[reference/rest-cheatsheet § Tutorial dataset loader](../reference/rest-cheatsheet.md#tutorial-dataset-loader).

## What you get in XNAT

- Project `XNAT_TUTORIAL_DICOM` (created if missing).
- 1 subject, 1 MR session, 1 scan.
- Scan-level `DICOM` resource with 14 files.
- Auto-archive enabled — if the loader leaves rows in the prearchive,
  archive them manually.

## Lessons that use this dataset

- Intro: [02 XNAT hierarchy](../intro/02-xnat-hierarchy.md),
  [03 DICOM import](../intro/03-dicom-import-archive.md),
  [04 DICOM headers](../intro/04-dicom-headers.md),
  [05 viewer basics](../intro/05-viewer-basics.md),
  [07 search and export](../intro/07-search-and-export.md).
- Intermediate: [01 dcm2niix](../intermediate/01-dcm2niix.md),
  [05 container basics](../intermediate/05-container-basics.md),
  [07 dynamic data type — QC review variant](../intermediate/07-dynamic-datatype.md).
