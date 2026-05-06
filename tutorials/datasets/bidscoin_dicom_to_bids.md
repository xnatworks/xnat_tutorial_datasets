# `bidscoin_dicom_to_bids` — BIDScoin tutorial DICOM

Curated `sub-001` raw DICOM subset from the public BIDScoin tutorial
data, with anatomical, single-band and multi-band BOLD, SBRef, and GRE
fieldmap series. Use it to walk the **complete** DICOM-to-BIDS pipeline
inside XNAT: protocol naming → mapping → conversion → materialised BIDS
tree → MRIQC.

| | |
|---|---|
| Modality | MR (multi-sequence) |
| Format | DICOM, ~712 files |
| License | BIDScoin tutorial materials |
| Source | [BIDScoin tutorial](https://bidscoin.readthedocs.io/en/stable/tutorial.html) |
| Plugin id | `bidscoin_dicom_to_bids` |
| Default project | `XNAT_TUTORIAL_BIDSCOIN` |
| Subjects | 1 (`sub-001`) |
| Sessions | 1 |
| Series | 11 (incl. localiser, scout, T1w MPRAGE, BOLD, SBRef, fieldmap pairs) |
| Archive size | 51 MB ZIP |

## Load it

UI — **Tools → Tutorial Datasets**, find `bidscoin_dicom_to_bids`,
**Prepare**.

REST — see
[reference/rest-cheatsheet § Tutorial dataset loader](../reference/rest-cheatsheet.md#tutorial-dataset-loader).

## What you get in XNAT

- Project `XNAT_TUTORIAL_BIDSCOIN`.
- 1 subject, 1 MR session, 11 scans. Expected scan-by-role mapping:

  | Scan | Role |
  |---|---|
  | `1`, `2` | localiser / scout (excluded from BIDS) |
  | `7` | T1w MPRAGE |
  | `47` / `48` | functional SBRef / BOLD |
  | `49` / `50` | fieldmap magnitude / phase |
  | `59` / `60` | multi-echo SBRef / BOLD |
  | `61` / `62` | multi-echo fieldmap pairs |

- A behavioural sidecar folder is preserved as non-DICOM tutorial
  metadata. With `skip unusable = true` on conversion, it is skipped
  automatically.

## Lessons that use this dataset

- Intermediate: [04 DICOM to BIDS](../intermediate/04-dicom-to-bids.md).
- Advanced: [01 complete BIDS workflow](../advanced/01-complete-bids.md)
  (this is the primary source).
- Intro: usable for [02 XNAT hierarchy](../intro/02-xnat-hierarchy.md)
  if you want a multi-scan example, but `tcia_dicom_intro` is faster.
