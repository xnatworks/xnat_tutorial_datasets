# XNAT Tutorials

A pick-and-mix catalog of XNAT walkthroughs. Lessons are grouped by level
so you can pull together a workshop agenda from the lessons that fit
your audience.

> A live, hosted version of these tutorials runs at
> **<https://tutorial.xnatworks.io>** — the easiest way to try a lesson
> without standing up your own XNAT.

If you are new to XNAT, start with [`GLOSSARY.md`](GLOSSARY.md) for the
vocabulary, then run
[intro/01-load-sample-data](intro/01-load-sample-data.md).

## Lesson catalog

### Intro — UI-only, 5–15 min, no terminal

| # | Lesson | Recommended dataset |
|---|---|---|
| 01 | [Load sample data into XNAT](intro/01-load-sample-data.md) | any |
| 02 | [Walk the XNAT hierarchy](intro/02-xnat-hierarchy.md) | `tcia_dicom_intro` |
| 03 | [DICOM import and archive](intro/03-dicom-import-archive.md) | `tcia_dicom_intro` |
| 04 | [Read DICOM headers](intro/04-dicom-headers.md) | `tcia_pseudo_phi_deid` |
| 05 | [Open a scan in the viewer](intro/05-viewer-basics.md) | `tcia_dicom_intro` |
| 06 | [Resource browser](intro/06-resource-browser.md) | `openneuro_flanker_bids` |
| 07 | [Search, filter, and export](intro/07-search-and-export.md) | `openneuro_flanker_bids` |
| 08 | [BIDS as an XNAT resource](intro/08-bids-as-resource.md) | `openneuro_flanker_bids` |
| 09 | [Atlases and overlays in NiiVue](intro/09-niivue-overlays.md) | `niivue_demo_images` |

### Intermediate — one tool or concept, 15–30 min

| # | Lesson | Recommended dataset |
|---|---|---|
| 01 | [DICOM to NIfTI with dcm2niix](intermediate/01-dcm2niix.md) | `tcia_dicom_intro` |
| 02 | [De-identification](intermediate/02-deidentification.md) | `tcia_pseudo_phi_deid` |
| 03 | [BIDS validation](intermediate/03-bids-validation.md) | `openneuro_flanker_bids` |
| 04 | [DICOM to BIDS](intermediate/04-dicom-to-bids.md) | `bidscoin_dicom_to_bids` |
| 05 | [Container launch basics](intermediate/05-container-basics.md) | `openneuro_flanker_bids` |
| 06 | [MRIQC assessor](intermediate/06-mriqc-assessor.md) | `openneuro_flanker_bids` |
| 07 | [Dynamic data type](intermediate/07-dynamic-datatype.md) | `openneuro_ds002551_metadata` |
| 08 | [Custom forms](intermediate/08-custom-forms.md) | `openneuro_ds002551_metadata` |
| 09 | [Preclinical MRI](intermediate/09-preclinical-mri.md) | `tcia_mouse_astrocytoma_mri` |

### Advanced — multi-step pipelines, 30 min+

| # | Lesson | Recommended dataset | GPU |
|---|---|---|:-:|
| 01 | [Complete BIDS workflow](advanced/01-complete-bids.md) | `bidscoin_dicom_to_bids` | — |
| 02 | [MONAI Bundle segmentation](advanced/02-monai-segmentation.md) | full-FOV abdomen+pelvis CT | yes |
| 03 | [TotalSegmentator vs MONAI](advanced/03-segmentation-comparison.md) | same CT as 02 | yes |
| 04 | [nnU-Net dataset builder](advanced/04-nnunet-dataset.md) | `msd_hippocampus_nnunet` | for inference |
| 05 | [RABIES rodent fMRI](advanced/05-rabies-rodent-fmri.md) | rodent rs-fMRI BIDS | optional |
| 06 | [FitLins group analysis](advanced/06-fitlins-group.md) | `fitlins_flanker_demo` | — |

### Admin — site configuration

| # | Lesson | Audience |
|---|---|---|
| 01 | [Event Service setup](admin/01-event-service.md) | Site admin |
| 02 | [Command enablement](admin/02-command-enablement.md) | Site admin |
| 03 | [Plugin loader — instructor view](admin/03-plugin-loader-admin.md) | Instructor / admin |

## Datasets

[`datasets/`](datasets/) — one reference card per dataset listing what
the data is, how to load it, and which lessons use it. The cards are
**references**, not tutorials.

## Reference

- [`GLOSSARY.md`](GLOSSARY.md) — XNAT vocabulary every lesson uses.
- [`reference/rest-cheatsheet.md`](reference/rest-cheatsheet.md) — `curl`
  recipes for the loader, resources, container launches, and the
  FreeSurfer license upload.
- [`reference/workshop-agendas.md`](reference/workshop-agendas.md) —
  pre-built running orders for 30 / 60 / 90-minute slots and themed
  tours.
- [`sources.yml`](sources.yml) — authoritative dataset source list.

## Trying it without standing up XNAT

The hosted demo at <https://tutorial.xnatworks.io> runs a current XNAT
with the tutorial plugin pre-loaded. It is the recommended starting
point for someone evaluating XNAT or trying a lesson before installing
anything locally.

## Running on your own XNAT

- XNAT 1.10 or newer.
- Tutorial plugin installed for one-click sample loading.
- Container Service plugin enabled for any lesson that runs containers.
- Docker host with GPU support where the lesson needs it (CUDA 12+
  recommended for MONAI / RABIES / nnU-Net).
- The relevant container command registered and enabled on the project
  (see [admin/02 command enablement](admin/02-command-enablement.md)).

REST examples reference these placeholders — set them in your shell or
substitute inline:

```
${XNAT_HOST}   ${XNAT_USER}   ${XNAT_PASS}
${PROJECT}     ${SESSION}     ${SCAN}     ${WRAPPER_ID}
```

## Lesson conventions

- Each intro / intermediate lesson answers four checkpoint questions:
  *what changes in XNAT*, *where to look*, *the success condition*, and
  *where to look first if it fails*. Advanced lessons drop the
  checkpoint scaffold for readability.
- Code blocks use bash. REST examples are copy-paste runnable once
  placeholders are set.
- Wiki cross-references link to public pages on
  [wiki.xnat.org](https://wiki.xnat.org).
- Container lessons pin Docker image tags. If a lesson refuses to
  resolve a command, suspect tag drift first
  ([admin/02](admin/02-command-enablement.md)).

## Maintenance

Lessons live under `intro/`, `intermediate/`, `advanced/`, and `admin/`.
Datasets they reference are mirrored under `../datasets/` (raw GitHub)
and pulled live from upstream when available. The plugin manifest at
[`sources.yml`](sources.yml) is the authoritative dataset source list.
