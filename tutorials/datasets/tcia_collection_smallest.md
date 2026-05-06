# `tcia_collection_smallest` — same data, collection-mode loader

The same QIN-PROSTATE-Repeatability series as
[`tcia_dicom_intro`](tcia_dicom_intro.md), but loaded by **collection
query** — the plugin asks NBIA for the smallest matching MR series at
load time rather than pinning a series UID.

The teaching value is the **mechanism**, not the data: tutorials are
software, and dataset selection has the same durability vs reproducibility
tradeoff as code dependencies.

| | |
|---|---|
| Modality | MR (prostate) |
| Format | DICOM |
| License | CC-BY 4.0 |
| Source | [TCIA QIN-PROSTATE-Repeatability](https://www.cancerimagingarchive.net/collection/qin-prostate-repeatability/) |
| Plugin id | `tcia_collection_smallest` |
| Plugin mode | `nbia_collection` (vs `nbia_series`) |
| Selection rule | smallest by file size, modality MR |
| Default project | `XNAT_TUTORIAL_DICOM` |

## Load it

UI — **Tools → Tutorial Datasets**, find `tcia_collection_smallest`,
**Prepare**.

REST — see
[reference/rest-cheatsheet § Tutorial dataset loader](../reference/rest-cheatsheet.md#tutorial-dataset-loader).

## What you get in XNAT

- 1 project, 1 subject, 1 MR session, 1 scan — whichever series is
  smallest right now.
- The exact series may change over time as TCIA reorganises the
  collection.

## Lessons that use this dataset

- This dataset is the **discussion point** for any DICOM-import lesson
  (intro [03](../intro/03-dicom-import-archive.md)) when you want to
  contrast pinned-UID loading vs collection-driven loading.
- Otherwise interchangeable with `tcia_dicom_intro` for any lesson that
  uses one DICOM scan.
