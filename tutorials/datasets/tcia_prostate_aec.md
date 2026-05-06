# `tcia_prostate_aec` — TCIA Prostate-AEC pelvis CT

A single-subject pelvis CT from the TCIA **Prostate-AEC** collection.
The session covers prostate + surrounding pelvic anatomy, so it has
enough field of view to produce a non-empty multi-organ
TotalSegmentator output, while staying small enough (~83 MB ZIP, 166
DICOM files) to load and segment in a live workshop slot.

| | |
|---|---|
| Modality | CT |
| Format | DICOM |
| License | CC-BY 4.0 |
| Source | [TCIA Prostate-AEC](https://www.cancerimagingarchive.net/collection/prostate-aec/) |
| Plugin id | `tcia_prostate_aec` |
| Default project | `XNAT_TUTORIAL_PROSTATE_AEC` |
| Subject | `Prostate-AEC-044` |
| Series | 1 (pelvis CT) |
| Files | 166 DICOM |
| Archive size | 83 MB ZIP |

## Load it

UI — **Tools → Tutorial Datasets**, find `tcia_prostate_aec`,
**Prepare**.

REST — see
[reference/rest-cheatsheet § Tutorial dataset loader](../reference/rest-cheatsheet.md#tutorial-dataset-loader).

## What you get in XNAT

- Project `XNAT_TUTORIAL_PROSTATE_AEC`.
- 1 subject (`Prostate-AEC-044`), 1 RX-simulation CT session.
- Scan-level `DICOM` resource on the pelvis CT series.

The source TCIA collection includes RTStruct and AI segmentation
scans alongside the CT. **Those are intentionally excluded from the
mirrored subset** so the dataset is a clean "CT in → segmentation
out" input for the TotalSegmentator lesson.

## Lessons that use this dataset

- Advanced: [05 TotalSegmentator](../advanced/05-totalsegmentator.md) —
  the canonical pairing.
- Pair with [intro/05 viewer basics](../intro/05-viewer-basics.md) for
  a DICOM-side viewer demo before running TS.
