# XNAT Tutorials

A pick-and-mix catalog of XNAT walkthroughs grouped by level. Pull
lessons à la carte for a workshop, a self-paced run, or a topic-specific
demo.

> A live, hosted version of these tutorials runs at
> **<https://tutorial.xnatworks.io>** — the easiest way to try a lesson
> without standing up your own XNAT.

If you are new to XNAT, start with [`tutorials/GLOSSARY.md`](tutorials/GLOSSARY.md)
for the vocabulary, then run
[tutorials/intro/01-load-sample-data](tutorials/intro/01-load-sample-data.md).

For dataset provenance, licenses, and the mirrored-files inventory, see
[`DATASETS.md`](DATASETS.md).

## Lesson catalog

### Intro — UI-only, 5–15 min, no terminal

| # | Lesson | Recommended dataset |
|---|---|---|
| 01 | [Load sample data into XNAT](tutorials/intro/01-load-sample-data.md) | any |
| 02 | [Walk the XNAT hierarchy](tutorials/intro/02-xnat-hierarchy.md) | `tcia_dicom_intro` |
| 03 | [DICOM import and archive](tutorials/intro/03-dicom-import-archive.md) | `tcia_dicom_intro` |
| 04 | [Read DICOM headers](tutorials/intro/04-dicom-headers.md) | `tcia_pseudo_phi_deid` |
| 05 | [Open a scan in the viewer](tutorials/intro/05-viewer-basics.md) | `tcia_dicom_intro` |
| 06 | [Resource browser](tutorials/intro/06-resource-browser.md) | `openneuro_flanker_bids` |
| 07 | [Search, filter, and export](tutorials/intro/07-search-and-export.md) | `openneuro_flanker_bids` |
| 08 | [BIDS as an XNAT resource](tutorials/intro/08-bids-as-resource.md) | `openneuro_flanker_bids` |
| 09 | [Atlases and overlays — Open with Workbench](tutorials/intro/09-niivue-overlays.md) | `niivue_demo_images` |

### Intermediate — one tool or concept, 15–30 min

| # | Lesson | Recommended dataset |
|---|---|---|
| 01 | [DICOM to NIfTI with dcm2niix](tutorials/intermediate/01-dcm2niix.md) | `tcia_dicom_intro` |
| 02 | [De-identification](tutorials/intermediate/02-deidentification.md) | `tcia_pseudo_phi_deid` |
| 03 | [BIDS validation](tutorials/intermediate/03-bids-validation.md) | `openneuro_flanker_bids` |
| 04 | [DICOM to BIDS](tutorials/intermediate/04-dicom-to-bids.md) | `bidscoin_dicom_to_bids` |
| 05 | [Container launch basics](tutorials/intermediate/05-container-basics.md) | `openneuro_flanker_bids` |
| 06 | [MRIQC assessor](tutorials/intermediate/06-mriqc-assessor.md) | `openneuro_flanker_bids` |
| 07 | [Dynamic data type](tutorials/intermediate/07-dynamic-datatype.md) | `openneuro_ds002551_metadata` |
| 08 | [Custom forms](tutorials/intermediate/08-custom-forms.md) | `openneuro_ds002551_metadata` |
| 09 | [Preclinical MRI](tutorials/intermediate/09-preclinical-mri.md) | `tcia_mouse_astrocytoma_mri` |

### Advanced — multi-step pipelines, 30 min+

| # | Lesson | Recommended dataset | GPU |
|---|---|---|:-:|
| 01 | [Complete BIDS workflow](tutorials/advanced/01-complete-bids.md) | `bidscoin_dicom_to_bids` | — |
| 02 | [nnU-Net dataset builder](tutorials/advanced/02-nnunet-dataset.md) | `msd_hippocampus_nnunet` | for inference |
| 03 | [RABIES rodent fMRI](tutorials/advanced/03-rabies-rodent-fmri.md) | rodent rs-fMRI BIDS | optional |
| 04 | [FitLins group analysis](tutorials/advanced/04-fitlins-group.md) | `fitlins_flanker_demo` | — |
| 05 | [TotalSegmentator](tutorials/advanced/05-totalsegmentator.md) | `tcia_prostate_aec` | yes |

### Admin — site configuration

| # | Lesson | Audience |
|---|---|---|
| 01 | [Event Service setup](tutorials/admin/01-event-service.md) | Site admin |
| 02 | [Command enablement](tutorials/admin/02-command-enablement.md) | Site admin |
| 03 | [Plugin loader — instructor view](tutorials/admin/03-plugin-loader-admin.md) | Instructor / admin |

## Dataset reference cards

[`tutorials/datasets/`](tutorials/datasets/) — one card per dataset
listing what the data is, how to load it, and which lessons use it. The
cards are **references**, not tutorials.

For upstream sources, licenses, and mirrored-file inventories see
[`DATASETS.md`](DATASETS.md).

## Reference

- [`tutorials/GLOSSARY.md`](tutorials/GLOSSARY.md) — XNAT vocabulary
  every lesson uses.
- [`tutorials/reference/rest-cheatsheet.md`](tutorials/reference/rest-cheatsheet.md) —
  `curl` recipes for the loader, resources, container launches, and the
  FreeSurfer license upload.
- [`tutorials/reference/workshop-agendas.md`](tutorials/reference/workshop-agendas.md) —
  pre-built running orders for 30 / 60 / 90-minute slots and themed
  tours.
- [`tutorials/sources.yml`](tutorials/sources.yml) — authoritative
  dataset source list consumed by the tutorial plugin.
- [`DATASETS.md`](DATASETS.md) — dataset inventory, provenance, and
  license details for every mirrored file.

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
  (see [admin/02 command enablement](tutorials/admin/02-command-enablement.md)).

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
  ([admin/02](tutorials/admin/02-command-enablement.md)).

## Maintenance

Lessons live under `tutorials/` (`intro/`, `intermediate/`, `advanced/`,
`admin/`). Datasets they reference are mirrored under `datasets/` (raw
GitHub) and pulled live from upstream when available.
[`tutorials/sources.yml`](tutorials/sources.yml) is the authoritative
source list. Verify mirrored files with:

```bash
shasum -a 256 -c SHA256SUMS
```
