# MRIQC assessor

**Level:** Intermediate · **Time:** 25 min · **Recommended dataset:** `openneuro_flanker_bids`
(also works with `bidscoin_dicom_to_bids` after conversion)

## Goal

Run a real BIDS app (MRIQC) against a session-level `BIDS` resource and
see the result land as an XNAT **assessor** linked to the session.

This lesson is the canonical "BIDS app on XNAT" pattern: setup helper
stages BIDS layout, main container runs the tool, wrapup container saves
outputs as an assessor.

## Preconditions

- Container Service installed.
- The `bids-mriqc-assessor` command enabled at the project.
- A session that has a session-level `BIDS` resource (skip if
  `openneuro_flanker_bids` is loaded; otherwise see
  [intermediate/04-dicom-to-bids](04-dicom-to-bids.md)).
- Image tags pinned to `nipreps/mriqc:24.0.2` and the curated wrapup
  `xnatworks/bids-assessor-wrapup:2.7`.

## Walkthrough

1. Open the session and confirm the `BIDS` resource exists.
2. Launch `bids-mriqc-assessor` from the session **Actions** menu.
3. In the launch dialog confirm:
   - Input is the session.
   - Wrapper uses `xnatworks/xnat2bids-setup:1.8:xnat2bids`.
   - Output handler resolves to `xnatworks/bids-assessor-wrapup:2.7`.
4. Submit. Open command history.
5. Watch three containers in order: setup helper → MRIQC → wrapup.
6. When the wrapup is `Complete`, return to the session.
7. Find the new MRIQC assessor in the assessors panel. Open it.
8. Open the assessor `DATA` resource. Inspect the MRIQC HTML reports and
   JSON outputs.

## Expected result

A new assessor of type roughly `bids:mriqcRunAssessorData`, with a
populated `DATA` resource containing MRIQC reports.

## Verify

You can open one MRIQC HTML report from inside the assessor's `DATA`
resource.

## If it does not work

- **MRIQC fails before running**: open setup-command logs. The
  `xnat2bids-setup` step needs to find a valid `BIDS` tree on the session
  resource.
- **MRIQC completes but no assessor appears**: open the wrapup container
  log. Confirm the wrapup is `bids-assessor-wrapup:2.7`. Earlier wrapup
  versions (`2.2`, `2.4`) are known to fail to register the assessor type.
- **Command resolution fails**: the BIDS assessor schema plugin may be
  missing. Check site **Administer → Plugin Settings**.

## Talking points

- This is the same pattern fMRIPrep, QSIPrep, and other BIDS apps use on
  XNAT. The setup / main / wrapup split keeps XNAT's archive layout out of
  the BIDS app's input.
- Discussion prompt: which artefacts belong to **acquisition** (DICOM scan
  resources), **conversion** (NIFTI + sidecars), **staged BIDS input**
  (session `BIDS` resource), and **analysis** (MRIQC `DATA` resource)?

Next: [advanced/01-complete-bids](../advanced/01-complete-bids.md) puts
mapping, conversion, validation, and MRIQC into one continuous walkthrough.
