# Simple XNAT Walkthrough Tutorials

These are self-service walkthroughs for teaching XNAT with the datasets in
this repository. Each tutorial recommends one or more datasets, then gives a
learner enough context, clicks, screenshots, and verification checks to work
without an instructor standing beside them.

The workflow instructions are adapted from the public XNAT wiki. Screenshots
are linked from wiki.xnat.org and are credited to the source page near each
walkthrough.

## Common Setup

If your XNAT has the tutorial dataset plugin installed:

1. Log in to XNAT.
2. Open the tutorial dataset loader from the XNAT menu or from a project
   Actions menu.
3. Choose the recommended dataset for the walkthrough.
4. Use the suggested project ID unless your course gives you a different one.
5. Click the prepare/load action and wait for the job to finish.
6. Open the created project from the XNAT project list.

REST equivalent for admins:

```bash
curl -u ${XNAT_USER}:${XNAT_PASS} -X POST \
  "${XNAT_HOST}/xapi/tutorials/datasets/${DATASET_ID}/prepare?projectId=${PROJECT}"
```

If the plugin is not installed, ask your administrator to preload the
recommended dataset into the named project, then begin at the first XNAT UI
step in the tutorial.

## Choose A Tutorial

| Tutorial | Recommended dataset |
|---|---|
| [XNAT Hierarchy](#xnat-hierarchy) | `tcia_dicom_intro` |
| [Project Setup And Roles](#project-setup-and-roles) | any prepared tutorial project |
| [DICOM Import And Archive](#dicom-import-and-archive) | `tcia_dicom_intro` |
| [DICOM Headers](#dicom-headers) | `tcia_pseudo_phi_deid` |
| [Viewer Basics](#viewer-basics) | `tcia_dicom_intro` |
| [Resource Browser](#resource-browser) | `openneuro_flanker_bids` |
| [Search, Filter, And Export](#search-filter-and-export) | `openneuro_flanker_bids` |
| [Provenance And Source Tracking](#provenance-and-source-tracking) | any tutorial dataset |
| [DICOM To NIfTI](#dicom-to-nifti) | `tcia_dicom_intro` |
| [De-Identification](#de-identification) | `tcia_pseudo_phi_deid` |
| [BIDS Layout](#bids-layout) | `openneuro_flanker_bids` |
| [BIDS Validation](#bids-validation) | `openneuro_flanker_bids` |
| [DICOM To BIDS](#dicom-to-bids) | `bidscoin_dicom_to_bids` |
| [Container Launch Basics](#container-launch-basics) | `openneuro_flanker_bids` |
| [Assessor Outputs](#assessor-outputs) | `openneuro_flanker_bids` |
| [Preclinical Imaging](#preclinical-imaging) | `tcia_mouse_astrocytoma_mri` |
| [Dynamic Datatype Intro](#dynamic-datatype-intro) | `openneuro_ds002551_metadata` |
| [Dynamic Form Curation](#dynamic-form-curation) | `openneuro_ds002551_metadata` |
| [Dynamic Datatype Search](#dynamic-datatype-search) | `openneuro_ds002551_metadata` |
| [Atlas And Overlay Viewing](#atlas-and-overlay-viewing) | `niivue_demo_images` |
| [Segmentation Outputs](#segmentation-outputs) | `msd_hippocampus_nnunet` |
| [Group Analysis Concepts](#group-analysis-concepts) | `fitlins_flanker_demo` |
| [fMRIPrep Or QSIPrep Setup](#fmriprep-or-qsiprep-setup) | `openneuro_flanker_bids` |
| [Event Service Setup](#event-service-setup) | any small DICOM or BIDS project |
| [Site Admin Basics](#site-admin-basics) | any tutorial project |

## Practical Use Cases

Use this table when a learner wants a concrete task instead of a feature
tour. Each use case starts with a downloaded tutorial dataset, follows the
relevant wiki workflow, and ends with something the learner can inspect in
XNAT.

| Use case | Real dataset | Wiki-backed walkthrough |
|---|---|---|
| Find a known project, subject, or session without browsing every table. | `openneuro_flanker_bids` or `tcia_dicom_intro` | [Search, Filter, And Export](#search-filter-and-export) |
| Download a small metadata file from a project resource. | `openneuro_flanker_bids` | [Resource Browser](#resource-browser), [Search, Filter, And Export](#search-filter-and-export) |
| Import DICOM and confirm it archived into the expected project, subject, session, scan, and resource. | `tcia_dicom_intro` | [DICOM Import And Archive](#dicom-import-and-archive), [XNAT Hierarchy](#xnat-hierarchy) |
| Inspect DICOM headers before deciding whether data is safe to share. | `tcia_pseudo_phi_deid` | [DICOM Headers](#dicom-headers), [De-Identification](#de-identification) |
| Convert an imaging scan into NIfTI and check where the derived files land. | `tcia_dicom_intro` | [DICOM To NIfTI](#dicom-to-nifti), [Assessor Outputs](#assessor-outputs) |
| Explain a BIDS dataset as a project resource with participants, sidecars, events, and NIfTI files. | `openneuro_flanker_bids` | [BIDS Layout](#bids-layout), [BIDS Validation](#bids-validation) |
| Launch a small processing command and inspect logs, outputs, and assessor resources. | `openneuro_flanker_bids` | [Container Launch Basics](#container-launch-basics), [Assessor Outputs](#assessor-outputs) |
| Add project-specific structured metadata without writing a custom plugin. | `openneuro_ds002551_metadata` | [Dynamic Datatype Intro](#dynamic-datatype-intro), [Dynamic Form Curation](#dynamic-form-curation) |
| Search and export curated metadata that was not part of the default XNAT schema. | `openneuro_ds002551_metadata` | [Dynamic Datatype Search](#dynamic-datatype-search) |
| Enable automation on a training site and identify a safe trigger/action pair. | `tcia_dicom_intro` or `openneuro_flanker_bids` | [Event Service Setup](#event-service-setup) |
| Open image overlays, atlas data, or segmentation masks in the viewer. | `niivue_demo_images` or `msd_hippocampus_nnunet` | [Atlas And Overlay Viewing](#atlas-and-overlay-viewing), [Segmentation Outputs](#segmentation-outputs) |
| Explain how a group analysis result differs from one subject's imaging session. | `fitlins_flanker_demo` | [Group Analysis Concepts](#group-analysis-concepts) |

## XNAT Hierarchy

Recommended dataset: `tcia_dicom_intro`

Also works with: `tcia_mouse_astrocytoma_mri`

Time: 10 minutes

Wiki source: [Understanding the XNAT Data Model](https://wiki.xnat.org/documentation/understanding-the-xnat-data-model)

![XNAT data model](https://wiki.xnat.org/__attachments/6470089/xnat-data-model.png?inst-v=60e1f417-9086-4f2b-9581-3db348fd3884)

Goal: Understand how XNAT organizes data.

Walkthrough:

1. Prepare `tcia_dicom_intro` into `XNAT_TUTORIAL_DICOM`.
2. Open the project page.
3. Find the subject list and open the first subject.
4. Open that subject's imaging session.
5. Find the scan table.
6. Open the first scan.
7. Open the scan's file/resource area and find the `DICOM` resource.
8. Compare what you see to the diagram above: project, subject, image
   session, scan, and resource.

Expected result: You can point to each level in XNAT and explain what lives
under it.

Verify: The project has at least one subject, one image session, one scan,
and one scan-level file resource.

## Project Setup And Roles

Recommended dataset: any prepared tutorial project

Time: 10 minutes

Wiki sources:

- [How To Use XNAT](https://wiki.xnat.org/documentation/how-to-use-xnat)
- [Understanding the XNAT Data Model](https://wiki.xnat.org/documentation/understanding-the-xnat-data-model)

![XNAT data model](https://wiki.xnat.org/__attachments/6470089/xnat-data-model.png?inst-v=60e1f417-9086-4f2b-9581-3db348fd3884)

Goal: Understand that project membership controls what a user can do.

Walkthrough:

1. Open a prepared tutorial project.
2. Review the project title, ID, description, and accessibility.
3. Open the project settings or manage area.
4. Find the project users, groups, or access page.
5. Identify owners, members, and collaborators.
6. Return to the project page.
7. Open the project Actions menu and note which actions are visible to you.
8. If a classmate has different permissions, compare which actions differ.

Expected result: You understand that project owners can administer project
data, while members and collaborators may have narrower permissions.

Verify: You can name the project owner role and find the page where project
membership is managed.

## DICOM Import And Archive

Recommended dataset: `tcia_dicom_intro`

Also works with: `tcia_collection_smallest`, `tcia_pseudo_phi_deid`

Time: 15 minutes

Wiki sources:

- [Image Session Upload Methods in XNAT](https://wiki.xnat.org/documentation/image-session-upload-methods-in-xnat)
- [Using the Prearchive](https://wiki.xnat.org/documentation/using-the-prearchive)
- [How XNAT Scans DICOM to Map to Project/Subject/Session](https://wiki.xnat.org/documentation/how-xnat-scans-dicom-to-map-to-project-subject-ses)

![XNAT data model](https://wiki.xnat.org/__attachments/6470089/xnat-data-model.png?inst-v=60e1f417-9086-4f2b-9581-3db348fd3884)

Goal: See how incoming DICOM is routed into project, subject, session, scan,
and resource records.

Walkthrough:

1. Prepare `tcia_dicom_intro` into `XNAT_TUTORIAL_DICOM`.
2. Open the site Prearchive page.
3. Look for a row matching the tutorial project, subject, or session.
4. If the row is visible, inspect its status.
5. If the row is ready, select it and choose the archive/commit action.
6. If no row is visible, auto-archive probably already moved it to the
   archive.
7. Open `XNAT_TUTORIAL_DICOM`.
8. Open the subject, session, scan, and scan-level `DICOM` resource.

Expected result: You can explain the difference between data that is still
in prearchive and data that has been committed to the archive.

Verify: The tutorial project contains an archived imaging session with a
scan-level `DICOM` resource.

## DICOM Headers

Recommended dataset: `tcia_pseudo_phi_deid`

Also works with: `tcia_dicom_intro`, `reproin_dicom_to_bids`

Time: 15 minutes

Wiki sources:

- [Image Session Upload Methods in XNAT](https://wiki.xnat.org/documentation/image-session-upload-methods-in-xnat)
- [How XNAT Scans DICOM to Map to Project/Subject/Session](https://wiki.xnat.org/documentation/how-xnat-scans-dicom-to-map-to-project-subject-ses)

![XNAT data model](https://wiki.xnat.org/__attachments/6470089/xnat-data-model.png?inst-v=60e1f417-9086-4f2b-9581-3db348fd3884)

Goal: Inspect DICOM metadata and connect it to XNAT routing and archive
metadata.

Walkthrough:

1. Prepare `tcia_pseudo_phi_deid` into `XNAT_TUTORIAL_DEID`.
2. Open the project, subject, session, and CT scan.
3. Open the scan files or file manager.
4. Open a DICOM file's header or tags view.
5. Find fields such as `PatientName`, `PatientID`, `StudyDate`,
   `AccessionNumber`, `Modality`, and `SeriesDescription`.
6. Compare these fields to the XNAT project, subject, session, and scan
   labels.
7. Decide which fields look like routing metadata and which fields look like
   sensitive patient metadata.

Expected result: You understand that XNAT indexes selected metadata, but the
DICOM headers remain the detailed source record.

Verify: You can find `Modality` and `SeriesDescription` in a DICOM header.

## Viewer Basics

Recommended dataset: `tcia_dicom_intro`

Also works with: `niivue_demo_images`, `tcia_mouse_astrocytoma_mri`

Time: 10 minutes

Wiki source: [Using the XNAT OHIF Viewer](https://wiki.xnat.org/xnat-ohif-viewer/using-the-xnat-ohif-viewer)

![OHIF contour panel](https://wiki.xnat.org/__attachments/6915268/Contour-based%20ROIs%20panel.png?inst-v=34d6c806-8b03-4dec-bc50-c927a5861397)

Goal: Open images from XNAT in the installed viewer and connect what you see
to the source scan.

Walkthrough:

1. Prepare `tcia_dicom_intro` into `XNAT_TUTORIAL_DICOM`.
2. Open the image session.
3. In the session Actions box, choose the image viewing action available on
   your site. On OHIF-enabled sites this is typically `View Images`.
4. Wait for the viewer to load.
5. Scroll through slices or frames.
6. If multiple scans are visible, select the scan you want to inspect.
7. Return to XNAT and open the scan table.
8. Match the viewer image to the scan ID and scan description.

Expected result: You can launch the viewer and identify which XNAT scan is
being displayed.

Verify: You can name the scan ID that produced the image you viewed.

## Resource Browser

Recommended dataset: `openneuro_flanker_bids`

Also works with: all prepared datasets

Time: 10 minutes

Wiki sources:

- [Strategies for XNAT Image Data Storage](https://wiki.xnat.org/documentation/strategies-for-xnat-image-data-storage)
- [How To Download Image Data From XNAT Projects](https://wiki.xnat.org/documentation/how-to-download-image-data-from-xnat-projects)

![Manage Files dialog](https://wiki.xnat.org/__attachments/6466928/image2016-12-27%2018%3A4%3A14.png?inst-v=1431b15b-aaac-488b-8bbd-8137000e0a26)

Goal: Understand resources as named file collections attached to XNAT
objects.

Walkthrough:

1. Prepare `openneuro_flanker_bids` into `XNAT_TUTORIAL_BIDS`.
2. Open the project page.
3. Find project-level resources and open the `BIDS` resource.
4. Open `dataset_description.json`.
5. Return to the project and open a subject or session if present.
6. Open a scan resource or session resource.
7. Compare where the file lives: project resource, session resource, or scan
   resource.
8. Use Manage Files, if available, to browse files in a tree view.

Expected result: You understand that resources are durable named file
collections attached at different levels of the XNAT hierarchy.

Verify: You can open one project resource file and one scan or session
resource file.

## Search, Filter, And Export

Recommended dataset: `openneuro_flanker_bids`

Also works with: `tcia_dicom_intro`, `openneuro_ds002551_metadata`

Time: 15 minutes

Wiki sources:

- [Using the Standard Search](https://wiki.xnat.org/documentation/using-the-standard-search)
- [Working with Scan Listings](https://wiki.xnat.org/documentation/working-with-scan-listings)
- [How To Download Image Data From XNAT Projects](https://wiki.xnat.org/documentation/how-to-download-image-data-from-xnat-projects)

![XNAT standard search box](https://wiki.xnat.org/__attachments/6457078/Screen%20Shot%202016-12-01%20at%203.47.47%20PM.png?inst-v=7ebd0c4b-509b-428e-9629-7fcd8b6dafb6)

![Standard search disambiguation results](https://wiki.xnat.org/__attachments/6457078/Screen%20Shot%202016-12-01%20at%203.49.14%20PM.png?inst-v=7ebd0c4b-509b-428e-9629-7fcd8b6dafb6)

![Imaging data download UI](https://wiki.xnat.org/__attachments/6466928/xnat-download-images.png?inst-v=1431b15b-aaac-488b-8bbd-8137000e0a26)

Goal: Find data using the top-bar standard search, table filters, and a
small export or download.

Walkthrough:

1. Prepare `openneuro_flanker_bids`.
2. Use the top-bar search box to search for an exact project ID, subject
   label, session label, title, alias, or keyword from the prepared project.
3. If XNAT shows multiple matches, choose the matching project, subject, or
   experiment from the grouped results page.
4. Return to the XNAT home page and use the Projects, Subjects, MR, PET, or
   CT tab search controls for a broader table-based search.
5. Open the project page.
6. Use the project table or subject/session table filter to narrow the list.
7. Open a matching row.
8. Open the resource file list.
9. Download a small text file such as `dataset_description.json` or
   `participants.tsv`.
10. Return to the project page.
11. If your site has a download action, open it and review the session, format,
   scan type, and download method selections.

Expected result: You understand when top-bar search works best and when a
table search or filter is the better tool.

Verify: You can locate and open `participants.tsv` or an equivalent metadata
file.

## Provenance And Source Tracking

Recommended dataset: any tutorial dataset

Time: 10 minutes

Wiki sources:

- [Understanding the XNAT Data Model](https://wiki.xnat.org/documentation/understanding-the-xnat-data-model)
- [Viewing Command History and Logs](https://wiki.xnat.org/container-service/viewing-command-history-and-logs)

![Container detail provenance](https://wiki.xnat.org/__attachments/8292259/HistoryDetail2-1.png?inst-v=7ebd0c4b-509b-428e-9629-7fcd8b6dafb6)

Goal: Learn where to look for source files, import records, and processing
history.

Walkthrough:

1. Prepare any tutorial dataset.
2. Open the project resources.
3. Look for provenance, source, or tutorial metadata resources.
4. Open a provenance file if one exists.
5. Identify the dataset ID, source URL, import user, and import time.
6. If a container has been run, open command history.
7. Open the command detail page and review command line, inputs, outputs, and
   environment information.
8. Return to the project resource list and find where the output files landed.

Expected result: You understand that provenance is split across source
metadata, XNAT resources, and workflow/container history.

Verify: You can name the dataset source and one XNAT object or command that
created derived files.

## DICOM To NIfTI

Recommended dataset: `tcia_dicom_intro`

Also works with: `tcia_mouse_astrocytoma_mri`

Time: 15 minutes

Wiki sources:

- [Launching Containers from Commands](https://wiki.xnat.org/container-service/launching-containers-from-commands)
- [Viewing Command History and Logs](https://wiki.xnat.org/container-service/viewing-command-history-and-logs)

![Run Containers action](https://wiki.xnat.org/__attachments/8292336/Screen%20Shot%202021-10-01%20at%201.37.15%20PM.png?inst-v=60e1f417-9086-4f2b-9581-3db348fd3884)

Goal: Convert a DICOM scan to NIfTI with a scan-level container command.

Walkthrough:

1. Prepare `tcia_dicom_intro`.
2. Open the image session and identify a usable scan.
3. In the scan table, look for a `Run` or `Run Container` menu.
4. Select the dcm2niix command, if it is enabled on your project.
5. Review the launch dialog.
6. Keep defaults unless your course gives specific values.
7. Run the container.
8. Open command history and wait for completion.
9. Return to the scan and look for a new `NIFTI` output resource.
10. Open the output resource and confirm it contains a NIfTI file and usually
    a JSON sidecar.

Expected result: You see a scan-level NIfTI output created from DICOM.

Verify: The scan has a `NIFTI` resource containing a `.nii` or `.nii.gz` file.

## De-Identification

Recommended dataset: `tcia_pseudo_phi_deid`

Time: 20 minutes

Wiki sources:

- [How To Use XNAT: DICOM Anonymization](https://wiki.xnat.org/documentation/how-to-use-xnat)
- [Image Session Upload Methods in XNAT](https://wiki.xnat.org/documentation/image-session-upload-methods-in-xnat)

![XNAT data model](https://wiki.xnat.org/__attachments/6470089/xnat-data-model.png?inst-v=60e1f417-9086-4f2b-9581-3db348fd3884)

Goal: Use pseudo-PHI DICOM to verify anonymization thinking.

Walkthrough:

1. Prepare `tcia_pseudo_phi_deid`.
2. Open one DICOM file's header or tags view.
3. Find patient/study fields such as name, ID, date, accession number, and
   free-text descriptions.
4. Decide whether each field should be removed, replaced, shifted, or kept.
5. If you are an admin, open the site or project anonymization settings.
6. Compare the policy to the fields you found.
7. If your class has a disposable XNAT, re-import or archive with the policy
   enabled.
8. Re-open the headers and compare before/after values.

Expected result: You understand that anonymization must be verified against
actual headers and not assumed from a project setting alone.

Verify: You can name three DICOM fields affected by the anonymization policy.

## BIDS Layout

Recommended dataset: `openneuro_flanker_bids`

Time: 15 minutes

Wiki source: [Strategies for XNAT Image Data Storage](https://wiki.xnat.org/documentation/strategies-for-xnat-image-data-storage)

![XNAT data model](https://wiki.xnat.org/__attachments/6470089/xnat-data-model.png?inst-v=60e1f417-9086-4f2b-9581-3db348fd3884)

Goal: Inspect a BIDS dataset stored as an XNAT resource.

Walkthrough:

1. Prepare `openneuro_flanker_bids` into `XNAT_TUTORIAL_BIDS`.
2. Open the project-level `BIDS` resource.
3. Open `dataset_description.json`.
4. Open `participants.tsv`.
5. Navigate to `sub-01/anat` and find the T1w NIfTI.
6. Navigate to `sub-01/func` and find the BOLD NIfTI and events TSV.
7. Notice that the BIDS dataset is a resource file tree, not a DICOM scan
   table.
8. Explain how this differs from the scan-level `DICOM` resource in the DICOM
   tutorials.

Expected result: You can identify dataset-level, subject-level, and run-level
BIDS files.

Verify: You can find `dataset_description.json`, `participants.tsv`, one
`*_T1w.nii.gz`, and one `*_events.tsv`.

## BIDS Validation

Recommended dataset: `openneuro_flanker_bids`

Time: 15 minutes

Wiki sources:

- [Launching Containers from Commands](https://wiki.xnat.org/container-service/launching-containers-from-commands)
- [Viewing Command History and Logs](https://wiki.xnat.org/container-service/viewing-command-history-and-logs)

![Container execution log](https://wiki.xnat.org/__attachments/8292259/CommandHistory2.png?inst-v=7ebd0c4b-509b-428e-9629-7fcd8b6dafb6)

Goal: Run or review validation of a BIDS resource through Container Service.

Walkthrough:

1. Prepare `openneuro_flanker_bids`.
2. Confirm the project has a `BIDS` resource.
3. Open the project Actions menu.
4. Choose the BIDS validator command if it is enabled.
5. Review the launch dialog and confirm the BIDS input resource.
6. Run the command.
7. Open command history and wait for completion.
8. Open the output resource, commonly named `BIDS_validation`.
9. Open the validation report and review errors or warnings.

Expected result: You see that BIDS can be checked before running heavier
analysis.

Verify: The project has a validation output resource or a prepared validation
report.

## DICOM To BIDS

Recommended dataset: `bidscoin_dicom_to_bids`

Also works with: `reproin_dicom_to_bids`

Full tutorial: [Complete BIDS workflow](07-complete-bids-walkthrough.md)

Time: 20 minutes

Wiki sources:

- [Setup Commands](https://wiki.xnat.org/container-service/setup-commands)
- [Launching Containers from Commands](https://wiki.xnat.org/container-service/launching-containers-from-commands)

![Run Containers action](https://wiki.xnat.org/__attachments/8292336/Screen%20Shot%202021-10-01%20at%201.37.15%20PM.png?inst-v=60e1f417-9086-4f2b-9581-3db348fd3884)

Goal: Convert real ReproIn DICOM data into BIDS-shaped outputs, then understand
how a BIDS setup command stages those outputs for BIDS apps.

Walkthrough:

1. Prepare `bidscoin_dicom_to_bids`.
2. Open the session and inspect the scan list.
3. Open DICOM headers for several scans.
4. Find protocol, series, task, acquisition, direction, or run labels.
5. Run or review the `bids-mapping-generator` output.
6. Launch `dcm2bids-session-v16` with `skip unusable = true`.
7. Open command history and watch the conversion logs.
8. Confirm generated NIfTI and BIDS sidecar resources exist on usable scans.
9. Launch `bids-materialize` to save a session-level `BIDS` resource.
10. Open that `BIDS` resource and inspect the materialized tree.
11. Launch or review `bids-mriqc-assessor`; it should use
   `xnatworks/xnat2bids-setup:1.7:xnat2bids` to stage BIDS layout.
12. Open the generated assessor and inspect its output resource.

Expected result: You understand why DICOM stored in XNAT archive structure
needs both conversion and setup-command staging before BIDS apps run.

Verify: You can explain the difference between raw scan-level DICOM, generated
scan-level NIfTI/BIDS sidecars, setup-command staged BIDS layout, and BIDS app
assessor outputs.

## Container Launch Basics

Recommended dataset: `openneuro_flanker_bids`

Also works with: `tcia_dicom_intro`

Time: 15 minutes

Wiki source: [Launching Containers from Commands](https://wiki.xnat.org/container-service/launching-containers-from-commands)

![Run Containers action](https://wiki.xnat.org/__attachments/8292336/Screen%20Shot%202021-10-01%20at%201.37.15%20PM.png?inst-v=60e1f417-9086-4f2b-9581-3db348fd3884)

Goal: Launch a simple command from the correct XNAT context.

Walkthrough:

1. Prepare the recommended dataset.
2. Open the project, session, scan, or resource that the command expects.
3. Look for `Run Containers` in the Actions box or scan table.
4. Choose a command.
5. Review the launch dialog.
6. Confirm that required inputs are populated.
7. Enter optional parameters only if your course specifies them.
8. Submit the launch.
9. Open command history to monitor progress.
10. After completion, return to the source object and find the output
    resource.

Expected result: You understand that the launch context determines what XNAT
mounts into the container.

Verify: You can find the container output resource after the run.

## Assessor Outputs

Recommended dataset: `openneuro_flanker_bids`

Also works with: `reproin_dicom_to_bids`

Time: 15 minutes

Wiki sources:

- [Understanding the XNAT Data Model](https://wiki.xnat.org/documentation/understanding-the-xnat-data-model)
- [Viewing Command History and Logs](https://wiki.xnat.org/container-service/viewing-command-history-and-logs)

![Container detail provenance](https://wiki.xnat.org/__attachments/8292259/HistoryDetail2-1.png?inst-v=7ebd0c4b-509b-428e-9629-7fcd8b6dafb6)

Goal: Understand derived processing results as assessors and resources.

Walkthrough:

1. Prepare a BIDS-capable dataset.
2. Run or open a prepared QC/processing command result.
3. Open the source session.
4. Find the Assessors section.
5. Open the assessor created by the processing command.
6. Open its output resource, often named `DATA` or a tool-specific label.
7. Open a report, JSON file, or log file.
8. Open command history and compare the command's outputs to the assessor
   resource.

Expected result: You see that derived analyses can be stored as XNAT
assessors linked to source sessions.

Verify: The session has an assessor or you can identify where the processing
output resource landed.

## Preclinical Imaging

Recommended dataset: `tcia_mouse_astrocytoma_mri`

Also works with: `openneuro_ds002551_metadata`, `niivue_demo_images`

Time: 15 minutes

Wiki sources:

- [Understanding the XNAT Data Model](https://wiki.xnat.org/documentation/understanding-the-xnat-data-model)
- [Strategies for XNAT Image Data Storage](https://wiki.xnat.org/documentation/strategies-for-xnat-image-data-storage)

![XNAT data model](https://wiki.xnat.org/__attachments/6470089/xnat-data-model.png?inst-v=60e1f417-9086-4f2b-9581-3db348fd3884)

Goal: Show that non-human imaging still fits the same XNAT hierarchy.

Walkthrough:

1. Prepare `tcia_mouse_astrocytoma_mri`.
2. Open the project and image session.
3. Inspect scan metadata and DICOM headers.
4. Open the viewer if available.
5. Compare the mouse MRI to a human imaging example.
6. Identify metadata that matters for preclinical work: species, strain,
   anesthesia, field strength, coil, and atlas space.
7. Decide which metadata fields are available in XNAT by default and which
   would need custom fields or a dynamic datatype.

Expected result: You understand that the XNAT hierarchy is shared, but
preclinical metadata often needs additional project-specific fields.

Verify: You can name one imaging field and one metadata field that matter
for mouse MRI.

## Dynamic Datatype Intro

Recommended dataset: `openneuro_ds002551_metadata`

Time: 20 minutes

Wiki sources:

- [Adding a Dynamic Data Type](https://wiki.xnat.org/documentation/adding-a-dynamic-data-type)
- [Creating a Custom Form](https://wiki.xnat.org/documentation/creating-a-custom-form)

![Dynamic data types administration table](https://wiki.xnat.org/__attachments/210206721/Screenshot%202026-04-22%20at%204.56.49%E2%80%AFPM.png?inst-v=844972ab-e23e-4829-a091-4e5132bc47d1)

![Create dynamic data type dialog](https://wiki.xnat.org/__attachments/210206721/Screenshot%202026-04-21%20at%2011.48.16%E2%80%AFAM.png?inst-v=844972ab-e23e-4829-a091-4e5132bc47d1)

Goal: Create or review a simple dynamic datatype for metadata that does not
fit the default XNAT imaging fields.

Walkthrough:

1. Prepare `openneuro_ds002551_metadata`.
2. Open the project resource that contains `participants.tsv` and
   `participants.json`.
3. Identify fields worth searching later, such as sex, field strength, coil,
   breathing condition, or sedation.
4. Decide the XNAT object level for the metadata. Use a subject assessor for
   participant-level metadata, an image assessor for session-level review or
   QC, and avoid using dynamic datatypes for a new image modality.
5. As an admin on a training site, open Administration -> Data Types.
6. Review the existing registered and dynamic data types.
7. Click `Create Datatype`.
8. Choose the parent type, such as `subjectAssessorData` for participant
   metadata.
9. Enter a technical name such as `rodentMetadata`, then fill in singular and
   plural display names.
10. Leave advanced prefix and complex type settings alone unless your course
    specifically asks you to test them.
11. Save the datatype.
12. When prompted or when returning later, add a custom form for the new
    datatype so users can enter the tutorial metadata fields.

Expected result: You understand how a site admin can add a simple XNAT data
type without writing and deploying a custom plugin.

Verify: You can name three metadata fields that belong in a custom or dynamic
datatype for the rodent dataset.

## Dynamic Form Curation

Recommended dataset: `openneuro_ds002551_metadata`

Also works with: `tcia_mouse_astrocytoma_mri`

Time: 20 minutes

Wiki sources:

- [Adding a Dynamic Data Type](https://wiki.xnat.org/documentation/adding-a-dynamic-data-type)
- [Creating a Custom Form](https://wiki.xnat.org/documentation/creating-a-custom-form)

![Custom form builder](https://wiki.xnat.org/__attachments/6460611/Screenshot%202023-03-28%20at%2010.22.46%20AM.png?inst-v=844972ab-e23e-4829-a091-4e5132bc47d1)

Goal: Build or review a custom form that captures tutorial metadata for the
new dynamic datatype.

Walkthrough:

1. Start with a dynamic datatype created for the tutorial, or use a training
   site where an instructor has already created one.
2. Open Tools -> Custom Forms -> Manage Data Forms.
3. Click `Add New`.
4. Enter a short form title such as `Rodent Acquisition Metadata`.
5. Choose the dynamic datatype the form applies to.
6. Choose site-wide scope or restrict the form to the tutorial project.
7. In the builder, drag a text, number, select, or checkbox component into
   the form.
8. Label the component using a field from `participants.tsv`.
9. Repeat for two or three fields.
10. Save the form.
11. Open the target object and confirm the custom form appears when creating
    or editing that object.

Expected result: You can use a custom form to capture metadata that is not
part of the default XNAT UI.

Verify: The saved form appears in the form dashboard or on the target object's
edit/report page.

## Dynamic Datatype Search

Recommended dataset: `openneuro_ds002551_metadata`

Time: 15 minutes

Wiki sources:

- [Adding a Dynamic Data Type](https://wiki.xnat.org/documentation/adding-a-dynamic-data-type)
- [Creating a Custom Form](https://wiki.xnat.org/documentation/creating-a-custom-form)
- [Using the Standard Search](https://wiki.xnat.org/documentation/using-the-standard-search)

![Project tab search controls](https://wiki.xnat.org/__attachments/6457078/Screen%20Shot%202016-12-01%20at%203.57.25%20PM.png?inst-v=7ebd0c4b-509b-428e-9629-7fcd8b6dafb6)

![Search result data table](https://wiki.xnat.org/__attachments/6457078/Screen%20Shot%202016-12-01%20at%204.01.24%20PM.png?inst-v=7ebd0c4b-509b-428e-9629-7fcd8b6dafb6)

Goal: Confirm that custom metadata is useful only when it can be searched,
filtered, or exported.

Walkthrough:

1. Start with a prepared project that has custom form values.
2. Use standard search for an exact project, subject, or session label so you
   can quickly reach the right area of XNAT.
3. Open a project table, data listing, stored search, or advanced search.
4. Add columns from the custom form or datatype if the site exposes them.
5. Filter for one value, such as sex, field strength, coil, or breathing
   condition.
6. Open a matching record.
7. Confirm the form values are visible on the report page.
8. Export the table if export is available.

Expected result: You can find records using metadata that was not part of
the default XNAT schema.

Verify: You can locate one record using a custom metadata field.

## Atlas And Overlay Viewing

Recommended dataset: `niivue_demo_images`

Time: 15 minutes

Wiki source: [Using the XNAT OHIF Viewer](https://wiki.xnat.org/xnat-ohif-viewer/using-the-xnat-ohif-viewer)

![OHIF contour panel](https://wiki.xnat.org/__attachments/6915268/Contour-based%20ROIs%20panel.png?inst-v=34d6c806-8b03-4dec-bc50-c927a5861397)

Goal: Practice using image resources, templates, atlases, and overlays.

Walkthrough:

1. Prepare `niivue_demo_images`.
2. Open the project resource or session containing the NIfTI images.
3. Open a template image.
4. Open an atlas, label map, or overlay image.
5. Launch the installed viewer.
6. Load the base image and overlay if the viewer supports it.
7. Adjust opacity, labels, or synchronization settings if available.
8. Decide which file is anatomy, which file is atlas/label data, and which
   object level stores each file.

Expected result: You can distinguish base images from atlas or overlay
resources.

Verify: You can identify which file is the base image and which file is the
overlay or label map.

## Segmentation Outputs

Recommended dataset: `msd_hippocampus_nnunet`

Also works with: `niivue_demo_images`

Time: 20 minutes

Wiki sources:

- [Using the XNAT OHIF Viewer](https://wiki.xnat.org/xnat-ohif-viewer/using-the-xnat-ohif-viewer)
- [Drawing Contours with the XNAT OHIF Viewer](https://wiki.xnat.org/ml/drawing-contours-with-the-xnat-ohif-viewer)

![OHIF contour panel](https://wiki.xnat.org/__attachments/6915268/Contour-based%20ROIs%20panel.png?inst-v=34d6c806-8b03-4dec-bc50-c927a5861397)

Goal: Inspect labels, masks, or ROI-style outputs as derived data.

Walkthrough:

1. Prepare `msd_hippocampus_nnunet` or open a prepared segmentation project.
2. Open one subject and imaging session.
3. Find the source image resource.
4. Find the label, mask, segmentation, or ROI resource.
5. Open both in a viewer that supports overlays.
6. Toggle the label or segmentation display.
7. Compare source image, ground-truth label, and predicted mask if all are
   present.
8. Note where the derived file is stored in XNAT.

Expected result: You can distinguish source images, labels, predicted masks,
and viewer annotations.

Verify: You can open a label or segmentation file and identify its source
image.

## Group Analysis Concepts

Recommended dataset: `fitlins_flanker_demo`

Also works with: `openneuro_flanker_bids`

Time: 15 minutes

Wiki sources:

- [Understanding the XNAT Data Model](https://wiki.xnat.org/documentation/understanding-the-xnat-data-model)
- [Strategies for XNAT Image Data Storage](https://wiki.xnat.org/documentation/strategies-for-xnat-image-data-storage)

![XNAT data model](https://wiki.xnat.org/__attachments/6470089/xnat-data-model.png?inst-v=60e1f417-9086-4f2b-9581-3db348fd3884)

Goal: Understand the difference between a single subject workflow and a
group-level analysis dataset.

Walkthrough:

1. Prepare or open `fitlins_flanker_demo`.
2. Open the project subject list.
3. Count the subjects and sessions.
4. Open one subject and inspect its anatomical and functional resources.
5. Return to the project level.
6. Identify which files describe the whole dataset or model.
7. Decide where group-level outputs should live: project-level resource,
   project-level assessor, or another analysis object.
8. Compare this with the single-subject `openneuro_flanker_bids` dataset.

Expected result: You understand that group analysis depends on consistent
resources across many subjects, not one scan alone.

Verify: You can explain why a group-level output should not be attached to a
single scan.

## fMRIPrep Or QSIPrep Setup

Recommended dataset: `openneuro_flanker_bids`

Also works with: `reproin_dicom_to_bids`

Time: 15 minutes

Wiki sources:

- [Setup Commands](https://wiki.xnat.org/container-service/setup-commands)
- [Launching Containers from Commands](https://wiki.xnat.org/container-service/launching-containers-from-commands)

![Container detail provenance](https://wiki.xnat.org/__attachments/8292259/HistoryDetail2-1.png?inst-v=7ebd0c4b-509b-428e-9629-7fcd8b6dafb6)

Goal: Review preflight checks before launching a heavy BIDS app.

Walkthrough:

1. Prepare or open a BIDS-capable project.
2. Confirm the project has a `BIDS` resource.
3. Check whether the relevant command wrapper is enabled for the project.
4. Check required project resources, including `LICENSES/fs_license.txt` for
   FreeSurfer-based workflows.
5. Review memory, CPU, disk, and runtime expectations.
6. Open the command launch dialog but do not submit yet.
7. Review resolved inputs and output handlers.
8. Run only on a site where this command and dataset have already been
   validated.

Expected result: You understand why heavy BIDS apps need license, compute,
and data-layout checks before launch.

Verify: You can identify at least one missing requirement that should block a
run.

## Event Service Setup

Recommended dataset: any small DICOM or BIDS project

Time: 15 minutes

Wiki sources:

- [Enabling the XNAT Event Service](https://wiki.xnat.org/documentation/enabling-the-xnat-event-service)
- [Launching Containers from Commands](https://wiki.xnat.org/container-service/launching-containers-from-commands)

![Event Service admin menu item](https://wiki.xnat.org/__attachments/6460624/admin-menu.png?inst-v=60e1f417-9086-4f2b-9581-3db348fd3884)

![Enable Event Service toggle](https://wiki.xnat.org/__attachments/6460624/enable-event-service.png?inst-v=60e1f417-9086-4f2b-9581-3db348fd3884)

Goal: Enable Event Service on a training site and understand the prerequisites
for attaching automation to data events.

Walkthrough:

1. Use a disposable training site or a site where Event Service testing is
   approved.
2. Prepare a small project such as `tcia_dicom_intro`.
3. Log in as a site administrator.
4. Open Administer -> Event Service.
5. On Event Setup, set Event Service to enabled.
6. Open Event Service History and review whether any events have already been
   recorded.
7. If the class will use container actions, confirm Container Service is
   installed, the required image is installed, and the command is enabled for
   the tutorial project.
8. Identify one safe trigger and one safe action, such as notifying a curator
   after a session arrives.
9. Avoid subscriptions that launch heavy containers on every archived session
   unless queueing and resource limits have been configured.

Expected result: You understand that Event Service is a site-level capability
and that automated actions need careful scope, project, and resource controls.

Verify: You can name one trigger and one action.

## Site Admin Basics

Recommended dataset: any tutorial project

Time: 20 minutes

Wiki sources:

- [Adding a Dynamic Data Type](https://wiki.xnat.org/documentation/adding-a-dynamic-data-type)
- [Enabling the XNAT Event Service](https://wiki.xnat.org/documentation/enabling-the-xnat-event-service)
- [Getting Started with Container Service](https://wiki.xnat.org/container-service/getting-started)
- [Creating a Custom Form](https://wiki.xnat.org/documentation/creating-a-custom-form)

![Dynamic data types administration table](https://wiki.xnat.org/__attachments/210206721/Screenshot%202026-04-22%20at%204.56.49%E2%80%AFPM.png?inst-v=844972ab-e23e-4829-a091-4b73e1e30705)

![Event Service admin menu item](https://wiki.xnat.org/__attachments/6460624/admin-menu.png?inst-v=60e1f417-9086-4f2b-9581-3db348fd3884)

Goal: Tour the common admin surfaces that affect tutorial data and tutorial
runs.

Walkthrough:

1. Log in as an administrator or a user with the required manager roles.
2. Open Administration -> Data Types and review registered and dynamic
   datatype settings.
3. Open Tools -> Custom Forms -> Manage Data Forms and review existing forms.
4. Open Administer -> Event Service and confirm whether Event Service is
   enabled for this training site.
5. Open Container Service settings or Container Registry.
6. Confirm the relevant command is installed.
7. Open the target tutorial project.
8. Confirm commands are enabled at project scope where needed.
9. Check project resources for required files such as `LICENSES/fs_license.txt`.
10. Open command history and review one prior command if available.
11. Avoid changing production settings unless you are following a site-approved
    admin task.

Expected result: You understand the split between site configuration, project
configuration, and user workflow.

Verify: You can determine whether a missing tutorial command is a site
registration problem, a project enablement problem, or a missing resource
problem.
