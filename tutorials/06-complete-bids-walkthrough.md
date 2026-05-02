# Tutorial 06 — Complete BIDS Workflow Walkthrough

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
- BIDSconvertR sequence-map example for the BIDScoin tutorial data:
  <https://bidsconvertr.github.io/tutorial/>
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

## How To Teach It

This walkthrough is meant to be self-service. For each stage, tell the learner:

- **What this step does**: the job this step performs in the DICOM-to-BIDS
  workflow.
- **What to look for**: the XNAT object, resource, log line, or container status
  that proves the step is behaving correctly.
- **How to know it worked**: the concrete success condition before moving on.
- **What to check if it does not**: the first place to look when the result is
  missing or surprising.

The important concept is that BIDS in XNAT is built in layers. Raw DICOM stays
on scan resources. Conversion creates scan-level NIfTI and BIDS sidecars.
Materialization stages those scan resources into a session-level BIDS tree.
BIDS apps such as MRIQC then consume the staged tree through a setup command and
write outputs back as assessors/resources.

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
  `LICENSES/fs_license.txt`. Do not bundle the license in this repo; each user
  or site must add their own license before launching fMRIPrep or QSIPrep.

## Dataset Preparation

What this step does: creates the tutorial project, imports the BIDScoin DICOM
subset, writes tutorial provenance, and enables the curated BIDS container
profile for the project. This step gives the learner real source DICOM inside
XNAT before any BIDS conversion has happened.

1. Open XNAT as an administrator or a project owner.
2. Open the tutorial dataset loader.
3. Select `bidscoin_dicom_to_bids`.
4. Use project ID `XNAT_TUTORIAL_BIDSCOIN`.
5. Prepare the dataset and wait for import to finish.
6. Open project `XNAT_TUTORIAL_BIDSCOIN`.
7. Open the imported MR session.

What to look for:

- The loader reports that DICOM files were routed through the prearchive or
  imported into the archive.
- The project has one subject, one MR session, and eleven scan series.
- The project has tutorial provenance under project resources.
- Container setup reports mapping, conversion, materialize, and MRIQC commands
  as installed and project-enabled. A missing `LICENSES/fs_license.txt` is only
  a blocker for FreeSurfer-dependent workflows such as fMRIPrep/QSIPrep, not for
  mapping, DICOM-to-BIDS conversion, materialization, or MRIQC.

Expected scan inventory:

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

How to know it worked: the session is archived, visible under the project, and
the scan table matches the inventory above. If the loader says the session is
still in prearchive, open the prearchive and wait for auto-archive or manually
archive the row before launching containers.

## Inspect The Source Data

What this step does: connects XNAT's scan-level DICOM archive to the BIDS naming
decisions that will happen later. Learners should see that BIDS conversion is
not magic; it depends on DICOM metadata such as protocol names, series numbers,
and acquisition descriptions.

1. Open the scan table for the session.
2. Open scan `7`, then the DICOM resource.
3. Inspect the DICOM headers.
4. Find `SeriesNumber`, `ProtocolName`, and `SeriesDescription`.
5. Repeat for scans `48`, `49`, `60`, and `62`.

What to look for:

- Scan `7` is a structural T1w acquisition.
- Scan `48` and scan `60` are functional BOLD candidates.
- Scan `47` and scan `59` are SBRef acquisitions that support functional runs.
- Scans `49`/`50` and `61`/`62` are fieldmap acquisitions.
- Scans `1` and `2` are useful teaching examples of source data that should not
  be sent into the scientific BIDS analysis.

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

How to know it worked: the learner can point at a DICOM header field and explain
why the later BIDS output will contain `anat`, `func`, `fmap`, `bold`, `sbref`,
or `T1w` paths. If the scan table is empty or the DICOM resource is missing,
stop and fix the import before moving on.

## Set Up Containers

What this step does: checks that XNAT Container Service has the exact curated
command JSONs needed for the tutorial and that those wrappers are enabled on the
project. This is the preflight stage for reproducible teaching.

1. Open the tutorial dataset loader or the tutorial container setup panel.
2. Select the BIDS demo pipeline profile.
3. Check container status for `XNAT_TUTORIAL_BIDSCOIN`.
4. Install missing command JSONs if needed.
5. Enable the commands on the project.
6. Confirm the Docker image tags are pinned:
   - `xnatworks/bids-mapping-generator:1.7.4`
   - `xnatworks/dcm2bids-session:2.16`
   - `busybox:1.37.0`
   - `bids/validator:2.5.6`
   - `nipreps/mriqc:24.0.2`
   - `nipreps/fmriprep:25.2.5`
   - `pennlinc/qsiprep:1.1.1`

What to look for:

- The command JSONs are verified against the hashes in `containers/manifest.yml`.
- The MRIQC assessor command uses
  `xnatworks/xnat2bids-setup:1.7:xnat2bids`.
- The MRIQC assessor handler uses
  `xnatworks/bids-assessor-wrapup:2.5:bids-assessor-wrapup`.
- The BIDS validator assessor handler uses
  `xnatworks/bids-assessor-wrapup:2.6:bids-assessor-wrapup`.
- The orchestration named `Tutorial BIDS Demo Pipeline` is assigned to the
  project.

How to know it worked: the mapping, conversion, materialize, and MRIQC commands
are available from the session or project Actions menu. If MRIQC command
resolution fails, check the wrapper JSON first; older wrappers may point at
`bids-assessor-wrapup:2.2` or `2.4`, which should be replaced by the curated
`2.5` command JSON.

## Generate Or Review The BIDS Mapping

What this step does: inventories the XNAT scan list and writes a draft mapping
from scan metadata to BIDS targets. This is the translation layer between "what
the scanner called the series" and "where the series should live in BIDS."

1. Open the BIDScoin tutorial session page.
2. Open the Actions menu.
3. Launch `bids-mapping-generator`.
4. Use the session-level wrapper if prompted.
5. Save the generated mapping to the project configuration.
6. Open command history and wait for completion.
7. Inspect the output resource or project config.

What to look for:

- Container status reaches `Complete`.
- A mapping resource or project configuration entry is created for later
  conversion.
- Scan `7` is mapped as anatomical T1w.
- Scan `48` or scan `60` is mapped as functional BOLD.
- SBRef and fieldmap scans are represented in the mapping.
- Localizer/scout scans are excluded or marked so they will be skipped.

How to know it worked: the mapping has enough information for the conversion
container to decide which scans should become BIDS files. If the mapping treats
localizers as BIDS inputs or misses the T1w/BOLD series, review the source DICOM
metadata and mapping rules before converting.

Current live-demo note: use `dcm2bids-session:2.16` for this dataset. It
preserves echo labels for multi-echo BOLD and fieldmap outputs. If a conversion
reports a naming collision, treat that as a mapping problem to fix explicitly,
not as a successful partial conversion.

Reference comparison: the upstream BIDScoin tutorial identifies scans `48` and
`60` as functional MRI/BOLD runs, and scans `47` and `59` as their SBRef
partners. BIDScoin asks learners to edit the task labels into meaningful study
names such as `task-reward` and `task-stop`; an automated mapper should not
pretend it can infer those behavioral names from the scanner protocol alone.
For this tutorial, the automated pass is acceptable when the BIDS tree contains
T1w, BOLD, SBRef, and fieldmap files with distinct acquisition/run/echo labels.
Learners can then edit the BIDS map to replace generic task labels with the
study-specific names used in the BIDScoin reference exercise.

## Convert DICOM To BIDS Resources

What this step does: converts each usable scan's DICOM files into NIfTI images
and BIDS sidecar JSON resources. This creates BIDS-shaped outputs on scan
resources, but it does not yet create one complete session-level BIDS folder.

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

What to look for:

- Container status reaches `Complete`.
- Logs show conversion for usable image scans.
- Logs may show skipped localizer/scout or non-image folders when
  `skip unusable = true`; that is acceptable when the scientific scans convert.
- Scan resources now include generated NIfTI files and BIDS sidecar JSON files.

How to know it worked: open the usable converted scans and confirm generated
NIfTI and BIDS JSON resources exist. For the BIDScoin tutorial, the scientific
T1w, BOLD, SBRef, and fieldmap scans should produce BIDS-shaped outputs, while
localizer/scout scans may remain raw DICOM-only. If the command fails on a
single scan, inspect stderr for the actual `dcm2niix` message and check whether
that scan contains valid DICOM image files.

## Materialize The BIDS Resource

What this step does: uses the setup command to assemble scan-level NIfTI/BIDS
resources into a normal BIDS directory tree, then saves that tree as a
session-level `BIDS` resource. This is the bridge between XNAT's archive layout
and BIDS app input expectations.

1. Open the same session.
2. Launch `bids-materialize`.
3. Confirm the wrapper uses
   `xnatworks/xnat2bids-setup:1.7:xnat2bids`.
4. Run the command.
5. Open command history and wait for completion.
6. Open the session resources and find the `BIDS` resource.
7. Inspect `dataset_description.json`, `sub-*`, `ses-*`, `anat`, `func`, and
   any `fmap` paths generated by your mapping.

What to look for:

- A setup-helper container runs before the materialize container.
- The materialize container reaches `Complete`.
- The session has a `BIDS` resource.
- The `BIDS` resource contains `dataset_description.json` plus subject/session
  directories.
- Anatomical, functional, and any generated fieldmap files are grouped into
  BIDS folders rather than scattered across individual scan resources.

How to know it worked: the learner can open the `BIDS` resource and recognize a
BIDS directory tree. If the `BIDS` resource is empty or missing, verify that
scan-level NIfTI/BIDS resources were created by the conversion step and that the
materialize wrapper uses `xnat2bids-setup:1.7`.

## Validate The BIDS Dataset

What this step does: runs the BIDS validator against the staged BIDS layout
before expensive BIDS apps run. The validator checks whether the generated tree
is valid BIDS and the wrapup records the pass/fail result as a session-level
BIDS validator assessor.

1. Open the same session.
2. Launch `bids-validator-v2`.
3. Confirm the wrapper uses
   `xnatworks/xnat2bids-setup:1.7:xnat2bids`.
4. Run the command.
5. Open command history and wait for the main validator and wrapup containers
   to complete.
6. Open the generated BIDS validator assessor.
7. Open the `DATA` resource and inspect `validation_report.json`,
   `validation_report.txt`, and the readable HTML summary if present.

What to look for:

- The validator image is `bids/validator:2.5.6`.
- The command exits successfully even when the BIDS dataset has validation
  errors; pass/fail is recorded in the assessor metrics.
- The assessor has a PASS or FAIL status plus error and warning counts.
- The `DATA` resource preserves the raw validator JSON for troubleshooting.

How to know it worked: the session has a BIDS validator assessor with explicit
pass/fail, error count, warning count, and a non-empty `DATA` resource. If it
fails during command resolution, verify that the wrapup command
`xnatworks/bids-assessor-wrapup:2.6:bids-assessor-wrapup` is installed and that
the BIDS assessor schema plugin includes `bids:bidsValidatorRunAssessorData`.

## Run MRIQC Through The BIDS Setup Path

What this step does: runs a real BIDS app against the staged BIDS layout and
uses a wrapup container to create an XNAT assessor. This demonstrates the full
pattern for BIDS apps on XNAT: setup command stages input, main container runs
the app, wrapup stores outputs in the XNAT data model.

1. Open the same session.
2. Launch `bids-mriqc-assessor`.
3. Confirm its input is the session.
4. Confirm the wrapper uses
   `xnatworks/xnat2bids-setup:1.7:xnat2bids`.
5. Run MRIQC with conservative resources for a live tutorial.
6. Open command history.
7. Open the generated assessor.
8. Open the `DATA` resource and inspect the MRIQC reports or JSON outputs.

What to look for:

- A setup-helper container using `xnatworks/xnat2bids-setup:1.7` completes.
- The MRIQC container runs with image `nipreps/mriqc:24.0.2`.
- The MRIQC command environment includes `PROJECT`, `SESSION_LABEL`, and
  `SESSION_ID`.
- The output handler resolves
  `xnatworks/bids-assessor-wrapup:2.5:bids-assessor-wrapup`.
- The wrapup container reaches `Complete`.
- A new assessor appears under the session with xsi type similar to
  `bids:mriqcRunAssessorData`.
- The assessor has a `DATA` resource containing MRIQC reports and JSON outputs.

How to know it worked: the session has an MRIQC assessor with a non-empty
`DATA` resource. If the main MRIQC container completes but the assessor is
missing, inspect the wrapup container and confirm it used
`bids-assessor-wrapup:2.5`. If MRIQC fails before running, inspect setup-command
logs and verify the session has a valid `BIDS` tree.

Suggested discussion prompt: ask the learner to identify which artifacts belong
to raw acquisition (`DICOM` scan resources), conversion (`NIFTI` and BIDS
sidecars), staged BIDS input (`BIDS` session resource), and analysis output
(`MRIQC` assessor `DATA` resource).

## Optional: Run fMRIPrep With Your Own FreeSurfer License

What this step does: demonstrates the same BIDS-app pattern with fMRIPrep. This
is intentionally optional because fMRIPrep is slower than MRIQC and requires a
FreeSurfer license. The tutorial repository must not distribute that license,
but a learner or site administrator can add their own license to the XNAT
project before launch.

The fMRIPrep wrapper expects this exact project resource path:

```text
LICENSES/fs_license.txt
```

That means the project resource label must be `LICENSES`, and the uploaded file
must be named `fs_license.txt`. The wrapper mounts that resource at `/Project`
and launches fMRIPrep with:

```text
--fs-license-file /Project/fs_license.txt
```

Add the license through the XNAT UI:

1. Get your FreeSurfer license file from your own FreeSurfer registration.
2. Rename the file to `fs_license.txt` if it has another name.
3. Open the tutorial project.
4. Open the project resources or files panel.
5. Add a project resource named `LICENSES` if it does not already exist.
6. Upload `fs_license.txt` into the `LICENSES` resource.
7. Reopen the tutorial container setup status and confirm the missing license
   warning is gone.

Add the license through REST:

```bash
export XNAT_HOST=http://demo02.xnatworks.io
export XNAT_USER=admin
export XNAT_PASS=admin
export PROJECT=XNAT_TUTORIAL_BIDSCOIN

curl -u "${XNAT_USER}:${XNAT_PASS}" -X PUT \
  --data-binary @/path/to/your/fs_license.txt \
  "${XNAT_HOST}/data/projects/${PROJECT}/resources/LICENSES/files/fs_license.txt?overwrite=true"
```

After the license is present:

1. Confirm the session has a materialized `BIDS` resource.
2. Confirm BIDS validation passes or review any validator errors before
   spending time on fMRIPrep.
3. Launch `fmriprep-session-assessor`.
4. Use conservative tutorial flags such as:

   ```text
   --participant-label sub-01 --output-spaces MNI152NLin2009cAsym
   ```

5. Open command history and monitor setup, fMRIPrep, and wrapup containers.
6. Open the generated fMRIPrep assessor and inspect the `DATA` resource.

What to look for:

- The setup-helper container uses `xnatworks/xnat2bids-setup:1.7`.
- The fMRIPrep container uses `nipreps/fmriprep:25.2.5`.
- Command resolution includes `LICENSES/fs_license.txt`.
- fMRIPrep writes HTML reports and derivative files into the assessor `DATA`
  resource.

How to know it worked: the session has an fMRIPrep assessor with a non-empty
`DATA` resource and fMRIPrep reports for the participant. If launch fails during
command resolution, check the project resource label and file name first:
`LICENSES/fs_license.txt` must exist on the project.

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

### `dcm2bids-session` Fails With A Naming Collision

A naming collision means two converted files are trying to write the same final
BIDS path. With `dcm2bids-session:2.16`, this should fail loudly instead of
silently overwriting data. The failure may look like:

```text
FileExistsError: Collision detected: ..._bold.nii.gz already exists!
```

Fix the mapping before rerunning:

1. Identify the two source scans or echoes named in the error.
2. Add a distinguishing BIDS entity such as `echo`, `run`, `acq`, or `dir` in
   the BIDS map.
3. If one input should not be converted, mark that scan as `unusable` and rerun
   with `skip unusable = true`.
4. Rerun with `overwrite = true` only after deleting or intentionally replacing
   the previous generated resources.

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
