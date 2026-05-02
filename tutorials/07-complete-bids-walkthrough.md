# Complete BIDS Workflow Walkthrough

## Purpose

This tutorial takes a user from real DICOM data in XNAT to BIDS-shaped data
ready for BIDS-app style processing. It is intended to be the practical,
self-service BIDS workflow for the tutorial dataset collection.

Primary dataset: `bidscoin_dicom_to_bids`

Fallback already-BIDS dataset: `openneuro_flanker_bids`

Alternate DICOM dataset: `reproin_dicom_to_bids`

## Sources

- BIDScoin tutorial:
  <https://bidscoin.readthedocs.io/en/stable/tutorial.html>
- Container Service setup-command workflow:
  <https://wiki.xnat.org/container-service/setup-commands>
- Container launch workflow:
  <https://wiki.xnat.org/container-service/launching-containers-from-commands>
- Command history and logs:
  <https://wiki.xnat.org/container-service/viewing-command-history-and-logs>
- ReproIn example dataset:
  <https://heudiconv.readthedocs.io/en/latest/reproin.html>

## What Learners Will Do

1. Download a real DICOM tutorial dataset into XNAT.
2. Inspect the session, scans, and DICOM headers.
3. Generate or review a BIDS mapping.
4. Run DICOM-to-BIDS conversion.
5. Inspect the generated NIfTI and BIDS sidecar resources.
6. Run or review MRIQC using the BIDS setup-command path.
7. Diagnose common failures from the scan and command logs.

## Preconditions

- XNAT has Container Service installed.
- Tutorial data loader is installed and configured to use
  `https://raw.githubusercontent.com/xnatworks/xnat_tutorial_datasets/main`.
- The BIDS demo command set is installed and enabled for the tutorial project:
  - `bids-mapping-generator`
  - `dcm2bids-session-v16`
  - `bids-materialize`
  - `bids-mriqc-assessor`
- Optional heavy BIDS apps are installed only when the site is ready for them:
  - `fmriprep-session-assessor`
  - `qsiprep-session-assessor`
- FreeSurfer-based workflows require a project resource at
  `LICENSES/fs_license.txt`. Do not bundle the license in this repo.

## Dataset Preparation

1. Open XNAT as an administrator or a project owner.
2. Open the tutorial dataset loader.
3. Select `bidscoin_dicom_to_bids`.
4. Use project ID `XNAT_TUTORIAL_BIDSCOIN`.
5. Prepare the dataset and wait for import to finish.
6. Open project `XNAT_TUTORIAL_BIDSCOIN`.
7. Open the imported MR session.

Expected result: The project has one subject, one MR session, and these eleven
scan series:

| Scan | Expected role | Expected DICOM count |
|---|---|---:|
| `1` | localizer | 3 |
| `2` | scout | 128 |
| `7` | T1w MPRAGE | 192 |
| `47` | functional SBRef | 1 |
| `48` | functional BOLD | 10 |
| `49` | fieldmap magnitude echoes | 128 |
| `50` | fieldmap phase | 64 |
| `59` | multi-echo functional SBRef | 3 |
| `60` | multi-echo functional BOLD | 30 |
| `61` | multi-echo fieldmap magnitude echoes | 102 |
| `62` | multi-echo fieldmap phase | 51 |

The localizer is useful for teaching scan inspection, but it should not become
a scientific BIDS input.

## Inspect The Source Data

1. Open the scan table for the session.
2. Open scan `7`, then the DICOM resource.
3. Inspect the DICOM headers.
4. Find `SeriesNumber`, `ProtocolName`, and `SeriesDescription`.
5. Repeat for scans `48`, `49`, `60`, and `62`.

Expected source metadata:

| Scan | ProtocolName | BIDS intent |
|---|---|---|
| `7` | `t1_mprage_sag_ipat2_1p0iso` | anatomical T1w |
| `47` | `cmrr_2p4iso_mb8_TR0700` | functional SBRef |
| `48` | `cmrr_2p4iso_mb8_TR0700` | functional BOLD |
| `49` | `field_map_2p4iso` | fieldmap magnitude echoes |
| `50` | `field_map_2p4iso` | fieldmap phase |
| `59` | `cmrr_2p5iso_mb3me3_TR1500` | multi-echo functional SBRef |
| `60` | `cmrr_2p5iso_mb3me3_TR1500` | multi-echo functional BOLD |
| `61` | `field_map_2p5iso` | multi-echo fieldmap magnitude echoes |
| `62` | `field_map_2p5iso` | multi-echo fieldmap phase |

Verify: The DICOM headers explain why this is a BIDS teaching dataset. The
scanner protocol names already contain BIDS-like entities.

## Set Up Containers

1. Open the tutorial dataset loader or the tutorial container setup panel.
2. Select the BIDS demo pipeline profile.
3. Check container status for `XNAT_TUTORIAL_BIDSCOIN`.
4. Install missing command JSONs if needed.
5. Enable the commands on the project.
6. Confirm the Docker image tags are pinned:
   - `xnatworks/bids-mapping-generator:1.7.0`
   - `xnatworks/dcm2bids-session:2.14`
   - `busybox:1.37.0`
   - `nipreps/mriqc:24.0.2`
   - `nipreps/fmriprep:25.2.5`
   - `pennlinc/qsiprep:1.1.1`

Expected result: The mapping, conversion, and MRIQC commands are available
from the session or project Actions menu.

## Generate Or Review The BIDS Mapping

1. Open the BIDScoin tutorial session page.
2. Open the Actions menu.
3. Launch `bids-mapping-generator`.
4. Use the session-level wrapper if prompted.
5. Save the generated mapping to the project configuration.
6. Open command history and wait for completion.
7. Inspect the output resource or project config.

Expected result: The generated mapping recognizes anatomical, functional,
SBRef, fieldmap, and multi-echo functional scans. Localizers and scouts should
be excluded or skipped.

Verify: The mapping includes scan `7` as an anatomical target and scan `48` or
`60` as a functional target.

## Convert DICOM To BIDS Resources

1. Open the BIDScoin tutorial session page.
2. Launch `dcm2bids-session-v16`.
3. Use `skip unusable = true`.
4. Use `overwrite = false` for the first run.
5. Use `overwrite = true` only when intentionally rerunning after deleting or
   replacing prior output resources.
6. Use `series_description` as the scan-level mapping field unless your
   project mapping specifies another field.
7. Start the container.
8. Open command history and watch stdout/stderr.

Expected result: Each usable scan gets generated NIfTI and BIDS sidecar
resources in XNAT archive layout. This is not yet a single BIDS folder; the
materialization command handles that staging and saves the result.

Verify: Open scans `7`, `48`, `49`, `50`, `60`, and `62` and confirm generated
NIfTI and BIDS JSON resources exist.

## Materialize The BIDS Resource

1. Open the same session.
2. Launch `bids-materialize`.
3. Confirm the wrapper uses
   `xnatworks/xnat2bids-setup:1.7:xnat2bids`.
4. Run the command.
5. Open command history and wait for completion.
6. Open the session resources and find the `BIDS` resource.
7. Inspect `dataset_description.json`, `sub-*`, `ses-*`, `anat`, `func`, and
   `fmap` paths.

Expected result: The setup command stages the scan-level NIfTI/BIDS resources
into a normal BIDS tree, and the materialize wrapper saves that tree as a
session-level `BIDS` resource for inspection and downstream tutorials.

Verify: The session has a `BIDS` resource with a recognizable BIDS directory
tree.

## Run MRIQC Through The BIDS Setup Path

1. Open the same session.
2. Launch `bids-mriqc-assessor`.
3. Confirm its input is the session.
4. Confirm the wrapper uses
   `xnatworks/xnat2bids-setup:1.7:xnat2bids`.
5. Run MRIQC with conservative resources for a live tutorial.
6. Open command history.
7. Open the generated assessor.
8. Open the `DATA` resource and inspect the MRIQC reports or JSON outputs.

Expected result: The setup container stages the XNAT scan resources into BIDS
layout, MRIQC runs against that staged layout, and the wrapup stores outputs as
an assessor under the session.

Verify: The session has an MRIQC assessor with an output resource.

## Troubleshooting

### `dcm2niix` Fails On The `behavioural` Folder

The BIDScoin subset preserves a behavioral sidecar folder from the upstream
tutorial. It is not DICOM image data. If a command tries to convert that folder
directly, it should skip it or report that no DICOM images were found there.

Debug the command configuration rather than deleting imaging scans:

1. Confirm the image series still have DICOM resources.
2. Confirm `skip unusable = true`.
3. Confirm the command is launched from the imaging session, not from the
   project metadata resource.
4. Open command history and verify that image scans completed before any
   behavioral-folder warning.

### `dcm2niix` Fails On Scan `16`

This note applies when using alternate dataset `reproin_dicom_to_bids`.

The mirrored ReproIn source archive is not inherently broken. Local validation
against the mirrored archive showed scan `16` converts with dcm2niix and
contains:

- 4 DICOM files
- `SeriesNumber=16`
- `ProtocolName=func_task-rest`
- `SeriesDescription=func_task-rest`

If the XNAT container fails with a stack trace like:

```text
subprocess.CalledProcessError: Command '['dcm2niix', ... '/SCANS/16/DICOM']'
returned non-zero exit status 2
```

then debug the live XNAT archive content, not the source ZIP first:

1. Delete or reset `XNAT_TUTORIAL_REPROIN`.
2. Re-prepare `reproin_dicom_to_bids` from the tutorial loader.
3. Open scan `16` and confirm its DICOM resource has exactly 4 files.
4. Rerun `dcm2bids-session-v16`.
5. If rerunning in an existing project, use `overwrite = true` or delete prior
   generated NIfTI/BIDS resources before retrying.
6. If it still fails, open the command's stderr log from command history. The
   Python stack trace alone hides the actual dcm2niix error message.

### The MRIQC Command Cannot See BIDS

MRIQC needs BIDS layout, not raw XNAT scan folders. Confirm the MRIQC wrapper
uses the setup command:

```text
xnatworks/xnat2bids-setup:1.7:xnat2bids
```

If the setup command is missing or using an older image, reinstall the curated
command JSONs from the tutorial dataset repo.

### The Dataset Imports But No Session Is Archived

Open the prearchive. If the session is stuck there, archive it or reset the
project and prepare the dataset again. The tutorial project should be configured
for auto-archive.

## Backup Dataset Candidate

If the DICOM-to-BIDS conversion command is unavailable, use
`openneuro_flanker_bids` to teach downstream BIDS inspection, validation, and
MRIQC concepts. It starts from an already-BIDS project resource rather than raw
DICOM, so it does not replace the conversion exercise.
