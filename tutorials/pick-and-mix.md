# Pick-And-Mix XNAT Tutorial Catalog

This catalog treats tutorials as the primary teaching objects and datasets
as recommendations. A tutorial should name one or more recommended datasets,
while each dataset remains a reusable asset that can support many tutorials.

Use this page when planning a workshop agenda from the datasets already
available in this repository. It does not require changes to the XNAT
tutorial plugin.

For user-followable instructions, see
[`simple-walkthroughs.md`](simple-walkthroughs.md).

## Practical Use-Case Map

These are the practical tutorial tracks the dataset repo should support. A
good track starts with a real downloaded dataset, follows a public XNAT wiki
workflow, and ends with an inspectable result in XNAT.

| Use case | Dataset recommendation | Wiki workflow anchor | Tutorial entry point |
|---|---|---|---|
| Find the right project, subject, or session quickly. | `openneuro_flanker_bids`, `tcia_dicom_intro` | Standard search and table search. | [Search, Filter, And Export](simple-walkthroughs.md#search-filter-and-export) |
| Download a targeted metadata or image file. | `openneuro_flanker_bids`, `tcia_dicom_intro` | Project download and resource file browsing. | [Resource Browser](simple-walkthroughs.md#resource-browser) |
| Load DICOM and verify XNAT's project/subject/session/scan hierarchy. | `tcia_dicom_intro` | Upload, prearchive, archive, and data model. | [DICOM Import And Archive](simple-walkthroughs.md#dicom-import-and-archive) |
| Check DICOM headers and de-identification risk. | `tcia_pseudo_phi_deid` | DICOM header review and anonymization concepts. | [DICOM Headers](simple-walkthroughs.md#dicom-headers) |
| Convert DICOM to NIfTI and inspect derived resources. | `tcia_dicom_intro`, `tcia_mouse_astrocytoma_mri` | Container launch and command history. | [DICOM To NIfTI](simple-walkthroughs.md#dicom-to-nifti) |
| Walk from DICOM import through BIDS conversion and MRIQC outputs. | `bidscoin_dicom_to_bids`, `reproin_dicom_to_bids` | Setup commands, DICOM-to-BIDS conversion, and command history. | [Complete BIDS workflow](07-complete-bids-walkthrough.md) |
| Teach BIDS layout using real participants, sidecars, events, and NIfTI files. | `openneuro_flanker_bids` | Resource storage and container workflow. | [BIDS Layout](simple-walkthroughs.md#bids-layout) |
| Run a processing command and review outputs as assessors/resources. | `openneuro_flanker_bids`, `reproin_dicom_to_bids` | Container Service launch and logs. | [Container Launch Basics](simple-walkthroughs.md#container-launch-basics) |
| Add project-specific metadata without deploying a plugin. | `openneuro_ds002551_metadata` | Dynamic data type plus Custom Forms. | [Dynamic Datatype Intro](simple-walkthroughs.md#dynamic-datatype-intro) |
| Search curated metadata captured by a custom form or dynamic datatype. | `openneuro_ds002551_metadata` | Standard search and data tables. | [Dynamic Datatype Search](simple-walkthroughs.md#dynamic-datatype-search) |
| Enable Event Service and plan a safe automation trigger/action pair. | `tcia_dicom_intro`, `openneuro_flanker_bids` | Event Service administration. | [Event Service Setup](simple-walkthroughs.md#event-service-setup) |
| Inspect overlays, atlases, contours, and segmentation masks. | `niivue_demo_images`, `msd_hippocampus_nnunet` | OHIF viewer workflows. | [Atlas And Overlay Viewing](simple-walkthroughs.md#atlas-and-overlay-viewing) |
| Explain group-level analysis outputs. | `fitlins_flanker_demo` | XNAT resource model and analysis provenance. | [Group Analysis Concepts](simple-walkthroughs.md#group-analysis-concepts) |

## Tutorial File Convention

Each simple tutorial in this repo should include a short dataset block near
the top:

```yaml
recommended_datasets:
  - dataset_id: tcia_dicom_intro
    why: Tiny real DICOM series; fast and reliable for live XNAT basics.
  - dataset_id: tcia_pseudo_phi_deid
    why: Best small dataset for DICOM header and anonymization exercises.
```

For prose-only tutorials, the same information can be written as:

```text
Recommended dataset: tcia_dicom_intro
Also works with: tcia_collection_smallest, tcia_mouse_astrocytoma_mri
```

## Tutorial Topics

| Tutorial topic | Level | Recommended dataset(s) | What the learner does |
|---|---|---|---|
| XNAT hierarchy | Beginner | `tcia_dicom_intro`, `tcia_mouse_astrocytoma_mri` | Walk project -> subject -> session -> scan -> resource. |
| Project setup and roles | Beginner | Any tutorial project | Review project accessibility, owners, members, collaborators, and actions. |
| DICOM import and archive | Beginner | `tcia_dicom_intro`, `tcia_collection_smallest`, `tcia_pseudo_phi_deid` | Load DICOM, inspect prearchive behavior, confirm archive resources. |
| DICOM headers | Beginner | `tcia_dicom_intro`, `tcia_pseudo_phi_deid`, `bidscoin_dicom_to_bids`, `reproin_dicom_to_bids` | Open scan metadata and DICOM headers; identify patient, scanner, protocol, and series fields. |
| Viewer basics | Beginner | `tcia_dicom_intro`, `niivue_demo_images`, `tcia_mouse_astrocytoma_mri` | Open an image session or NIfTI resource in the installed viewer and compare pixels to XNAT metadata. |
| Resource browser | Beginner | All datasets | Inspect project, session, scan, assessor, and resource file layouts. |
| Search, filter, and export | Beginner | `tcia_dicom_intro`, `openneuro_flanker_bids`, `openneuro_ds002551_metadata` | Filter tables, find sessions or resources, and export/download small files. |
| Provenance and source tracking | Beginner | All datasets | Review source URLs, licenses, checksums, and imported project resources. |
| DICOM to NIfTI | Intermediate | `tcia_dicom_intro`, `tcia_mouse_astrocytoma_mri` | Run or discuss dcm2niix and inspect scan-level `NIFTI` output. |
| De-identification | Intermediate | `tcia_pseudo_phi_deid` | Compare PHI-shaped header fields before and after anonymization policy. |
| BIDS layout | Intermediate | `openneuro_flanker_bids` | Inspect `dataset_description.json`, `participants.tsv`, sidecars, events, and NIfTI paths. |
| BIDS validation | Intermediate | `openneuro_flanker_bids` | Run or review a BIDS validator container and inspect output resources. |
| DICOM to BIDS | Intermediate | `bidscoin_dicom_to_bids`, `reproin_dicom_to_bids` | Explain protocol naming, mapping generation, DICOM conversion, setup-command staging, MRIQC assessor outputs, and recovery from scan-level conversion failures. |
| Container launch basics | Intermediate | `openneuro_flanker_bids`, `tcia_dicom_intro` | Launch a simple container, follow job status, and inspect output resources. |
| Assessor outputs | Intermediate | `openneuro_flanker_bids`, `reproin_dicom_to_bids` | Show how MRIQC or pipeline results become XNAT assessors and resources. |
| Preclinical imaging | Intermediate | `tcia_mouse_astrocytoma_mri`, `openneuro_ds002551_metadata`, `niivue_demo_images` | Compare mouse and human imaging scale, metadata, and viewer behavior. |
| Dynamic datatype intro | Intermediate | `openneuro_ds002551_metadata` | Use tabular participant metadata to motivate a custom metadata datatype. |
| Dynamic form curation | Intermediate | `openneuro_ds002551_metadata`, `tcia_mouse_astrocytoma_mri` | Enter or review custom fields for subject/session curation. |
| Dynamic datatype search | Intermediate | `openneuro_ds002551_metadata` | Query, filter, or export values captured in a custom datatype. |
| Atlas and overlay viewing | Intermediate | `niivue_demo_images` | Open templates, atlases, label maps, and overlays in a viewer. |
| Segmentation outputs | Advanced | `msd_hippocampus_nnunet`, `niivue_demo_images` | Inspect label resources, predicted masks, and derived outputs. |
| Group analysis concepts | Advanced | `fitlins_flanker_demo`, `openneuro_flanker_bids` | Explain single-subject versus group-level analysis and downstream results. |
| fMRIPrep or QSIPrep setup | Advanced | `openneuro_flanker_bids`, `reproin_dicom_to_bids` | Review BIDS inputs, FreeSurfer license preflight, and heavy-container caveats. |
| Event Service setup | Advanced/Admin | Any small DICOM or BIDS project | Enable Event Service on a training site and plan safe trigger/action pairs. |
| Site admin basics | Admin | Any tutorial project | Review command enablement, plugin settings, project resources, and reset workflows. |

## Dataset Reuse

These are examples of how one recommended dataset can appear in multiple
tutorials. This is not a one-dataset-to-one-tutorial mapping.

| Dataset id | Tutorials it can support |
|---|---|
| `tcia_dicom_intro` | XNAT hierarchy, DICOM import, prearchive/archive, DICOM headers, viewer basics, resource browser, DICOM to NIfTI, container launch basics. |
| `tcia_collection_smallest` | NBIA collection selection, DICOM import mechanics, reproducibility versus upstream drift. |
| `tcia_pseudo_phi_deid` | DICOM headers, PHI inspection, anonymization rules, before/after de-identification QA. |
| `bidscoin_dicom_to_bids` | Complete DICOM-to-BIDS workflow, BIDScoin tutorial source data, anatomical/functional/SBRef/fieldmap conversion, MRIQC setup-command staging, assessor outputs. |
| `openneuro_flanker_bids` | BIDS layout, BIDS validation, project resources, task events, container launch basics, MRIQC/fMRIPrep concepts, group analysis bridge. |
| `reproin_dicom_to_bids` | ReproIn protocol naming, DICOM to BIDS, mapping generation, BIDS pipeline orchestration, assessor outputs. Current source archive passes per-series dcm2niix conversion; validate the live XNAT conversion path before using in class. |
| `niivue_demo_images` | Viewer basics, NIfTI resources, atlas overlays, label maps, multi-volume comparison, reference-image library concepts. |
| `tcia_mouse_astrocytoma_mri` | Preclinical DICOM, mouse MRI, human versus mouse scale comparison, DICOM to NIfTI, viewer basics. |
| `openneuro_ds002551_metadata` | Dynamic datatype intro, custom metadata forms, preclinical metadata curation, search/filter/export of non-imaging metadata. |
| `msd_hippocampus_nnunet` | Segmentation labels, training/test data concepts, nnU-Net inference, mask inspection, derived resources. |
| `fitlins_flanker_demo` | Multi-subject BIDS, group-level GLM concepts, FitLins, analysis provenance. |

## Example Agendas

### 30-Minute New User Tour

1. `tcia_dicom_intro`: XNAT hierarchy and DICOM resources.
2. `tcia_dicom_intro`: viewer basics.
3. `openneuro_flanker_bids`: BIDS layout as a project resource.

### 60-Minute Data Management Tour

1. `tcia_dicom_intro`: DICOM import and archive.
2. `tcia_pseudo_phi_deid`: anonymization and header review.
3. `openneuro_flanker_bids`: BIDS layout and validation.
4. `openneuro_ds002551_metadata`: dynamic datatype motivation.

### 90-Minute Research Workflow Tour

1. `tcia_dicom_intro`: archive and resource model.
2. `openneuro_flanker_bids`: BIDS dataset and validation.
3. `bidscoin_dicom_to_bids`: DICOM to BIDS concepts.
4. `niivue_demo_images`: viewer and overlay examples.
5. `openneuro_ds002551_metadata`: dynamic datatype and curation.

### Complete BIDS Workflow Tour

1. `bidscoin_dicom_to_bids`: prepare real BIDScoin tutorial DICOM into
   `XNAT_TUTORIAL_BIDSCOIN`.
2. Inspect the anatomical, functional, SBRef, and fieldmap scans and connect
   protocol names to BIDS targets.
3. Run `bids-mapping-generator` and review the saved mapping.
4. Run `dcm2bids-session-v16` and inspect generated scan-level NIfTI/BIDS
   resources.
5. Run or review `bids-mriqc-assessor`, then open the assessor `DATA` resource.
6. Use [Complete BIDS workflow](07-complete-bids-walkthrough.md) for the full
   self-service steps and troubleshooting notes.

### 90-Minute Practical Use-Case Tour

1. `tcia_dicom_intro`: find the project with standard search, then walk the
   hierarchy down to a DICOM resource.
2. `tcia_pseudo_phi_deid`: inspect DICOM header fields and identify what
   must be protected before sharing.
3. `openneuro_flanker_bids`: download `participants.tsv`, inspect BIDS paths,
   and review how a container would consume the resource.
4. `openneuro_ds002551_metadata`: add or review a dynamic datatype and custom
   form for searchable project-specific metadata.
5. `tcia_dicom_intro` or `openneuro_flanker_bids`: open Event Service on a
   training site and define one safe trigger/action pair.

### Admin-Oriented Tour

1. Any dataset: project roles, reset workflow, and provenance.
2. `tcia_pseudo_phi_deid`: anonymization policy check.
3. `openneuro_flanker_bids`: container command enablement and output resources.
4. `openneuro_ds002551_metadata`: dynamic datatype setup and governance.
5. Any small project: Event Service enablement and safe subscription scoping.

## Maintenance Notes

- Keep tutorial topics small. A good topic should fit in 10 to 20 minutes.
- Prefer reusing existing dataset IDs over adding new datasets.
- Prefer concrete use cases over feature tours. The learner should always
  inspect a real project, resource, header, form value, command history, or
  search result.
- Mark tutorials that need GPU, long-running containers, or license files as
  advanced or admin-facing.
- If a dataset supports a new topic, add it here even before any plugin UI
  consumes the mapping.
