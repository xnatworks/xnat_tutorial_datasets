# DICOM to BIDS

**Level:** Intermediate · **Time:** 30 min · **Recommended dataset:** `bidscoin_dicom_to_bids`
(also works with `reproin_dicom_to_bids`)

## Goal

Convert real DICOM into BIDS-shaped scan resources using the curated
mapping and conversion containers. Recognise the difference between
scan-level NIfTI / sidecar resources and a session-level `BIDS` tree.

For the full pipeline that adds materialisation, validation, and MRIQC,
see [advanced/01-complete-bids](../advanced/01-complete-bids.md).

## Preconditions

- Container Service installed.
- Commands enabled at the project: `bids-mapping-generator`,
  `dcm2bids-session-v17`, `bids-materialize`.
- Image tags pinned to `xnatworks/bids-mapping-generator:1.7.4` and
  `xnatworks/dcm2bids-session:2.17`.

## Walkthrough

1. Load `bidscoin_dicom_to_bids` into `XNAT_TUTORIAL_BIDSCOIN`.
2. Open the session. Confirm 11 scan rows; expected protocol/role mapping:

   | Scan | ProtocolName | BIDS intent |
   |---|---|---|
   | `7` | `t1_mprage_sag_ipat2_1p0iso` | anatomical T1w |
   | `47` / `48` | `cmrr_2p4iso_mb8_TR0700` | functional SBRef / BOLD |
   | `49` / `50` | `field_map_2p4iso` | fieldmap magnitude / phase |
   | `59` / `60` | `cmrr_2p5iso_mb3me3_TR1500` | multi-echo SBRef / BOLD |
   | `61` / `62` | `field_map_2p5iso` | multi-echo fieldmap |

   Scans `1` and `2` are localiser / scout — useful for inspection,
   excluded from BIDS conversion.

3. Open scan `7` and inspect the DICOM headers
   ([intro/04-dicom-headers](../intro/04-dicom-headers.md)). Note
   `ProtocolName`, `SeriesNumber`, `SeriesDescription`. These drive the
   mapping.
4. Launch `bids-mapping-generator` from the session **Actions** menu. Wait
   for `Complete`. Open the saved mapping resource or project config and
   confirm the T1w, BOLD, SBRef, and fieldmap scans are mapped, and that
   localisers are excluded.
5. Launch `dcm2bids-session-v17` with `skip unusable = true` and
   `overwrite = false`. Wait for `Complete`.
6. Open each science scan (T1w, BOLD, SBRef, fieldmap pairs). Confirm
   each has a generated NIfTI plus a BIDS sidecar JSON.
7. Launch `bids-materialize`. Confirm it uses the
   `xnatworks/xnat2bids-setup:1.8:xnat2bids` setup helper. Wait for
   `Complete`.
8. Open the session resources. A new `BIDS` resource holds
   `dataset_description.json` plus a normal BIDS tree.

## Expected output

- Scan-level: `NIFTI` + BIDS sidecar resources on every science scan.
- Session-level: a `BIDS` resource with subject / session / `anat` / `func`
  / `fmap` directories.

## Verify

Open the `BIDS` resource and locate one T1w NIfTI under
`sub-*/ses-*/anat/`, one BOLD NIfTI under `func/`, and a fieldmap
`magnitude1` / `phasediff` pair under `fmap/`.

## If it does not work

- **Naming collision** during conversion: two scans want the same BIDS
  filename. Edit the mapping to add `echo`, `run`, `acq`, or `dir`
  entities, or mark one scan as unusable.
- **dcm2niix fails on the `behavioural` folder** (BIDScoin only): the
  subset preserves a behavioural sidecar folder that is not DICOM. With
  `skip unusable = true` it should be skipped automatically. If the
  command fails first, confirm the skip flag and that the launch context
  is the imaging session, not the metadata resource.
- **Empty `BIDS` resource after materialisation**: re-check that scan-
  level NIfTI / sidecar resources exist, and that the materialise wrapper
  uses `xnat2bids-setup:1.8`.

## Reference

- [BIDScoin tutorial](https://bidscoin.readthedocs.io/en/stable/tutorial.html)
- [Container Service setup commands](https://wiki.xnat.org/container-service/setup-commands)
