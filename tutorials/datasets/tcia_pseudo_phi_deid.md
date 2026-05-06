# `tcia_pseudo_phi_deid` — TCIA Pseudo-PHI-DICOM-Data

The canonical teaching dataset for DICOM de-identification. Synthetic
PHI in the *places* PHI normally appears in real clinical DICOMs:
patient name, date of birth, accession number, free-text fields, private
tags. Use it to verify that anonymisation pipelines actually scrub
everything you think they scrub.

| | |
|---|---|
| Modality | CT |
| Format | DICOM |
| License | CC-BY 4.0 |
| Source | [TCIA Pseudo-PHI-DICOM-Data](https://www.cancerimagingarchive.net/collection/pseudo-phi-dicom-data/) |
| Plugin id | `tcia_pseudo_phi_deid` |
| Default project | `XNAT_TUTORIAL_DEID` |
| Files | 2 DICOM (intentionally tiny) |

## Load it

UI — **Tools → Tutorial Datasets**, find `tcia_pseudo_phi_deid`,
**Prepare**.

REST — see
[reference/rest-cheatsheet § Tutorial dataset loader](../reference/rest-cheatsheet.md#tutorial-dataset-loader).

## What you get in XNAT

- Project `XNAT_TUTORIAL_DEID`.
- 1 subject, 1 CT scan, 2 DICOM files.
- Each file's headers contain pseudo-PHI in the standard PHI tag
  positions.

## Lessons that use this dataset

- Intro: [04 DICOM headers](../intro/04-dicom-headers.md).
- Intermediate: [02 de-identification](../intermediate/02-deidentification.md).
