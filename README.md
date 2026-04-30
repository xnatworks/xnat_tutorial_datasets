# XNAT Tutorial Datasets

Fallback dataset mirror for the XNAT tutorial plugins.

The `xnat_tutorial_plugin` uses the original public sources first. If the
primary source is unavailable or too slow for a workshop, the plugin falls back
to raw files in this repository under:

`https://raw.githubusercontent.com/xnatworks/xnat_tutorial_datasets/main`

These files are small workshop subsets, not full public datasets.

## Dataset Inventory

| Fallback path | Original source | Credit / license |
| --- | --- | --- |
| `datasets/openneuro/ds000102/` | OpenNeuro / OpenfMRI ds000102 Flanker task | Data from OpenfMRI/OpenNeuro accession `ds000102`; license reported by dataset metadata as PDDL |
| `datasets/xnat-tutorial/tcia_dicom_intro/archive.zip` | TCIA QIN-PROSTATE-Repeatability | The Cancer Imaging Archive, QIN-PROSTATE-Repeatability collection; Creative Commons Attribution 4.0 International |
| `datasets/xnat-tutorial/tcia_collection_smallest/archive.zip` | TCIA QIN-PROSTATE-Repeatability | Same tiny series as `tcia_dicom_intro`, used as the fallback for collection-mode selection |
| `datasets/xnat-tutorial/tcia_mouse_astrocytoma_mri/archive.zip` | TCIA Mouse-Astrocytoma | The Cancer Imaging Archive, Mouse-Astrocytoma collection; Creative Commons Attribution 3.0 Unported |
| `datasets/grouplevel/msd/Task04_Hippocampus.tar` | Medical Segmentation Decathlon / MONAI mirror | MSD Task04 Hippocampus; CC-BY-SA 4.0 |

The group-level tutorial source catalog is in
`datasets/grouplevel/sources.yml`. Only the small MSD Hippocampus archive is
mirrored as a normal GitHub raw file. Other referenced group-level sources are
hundreds of MB to tens of GB and need GitHub release assets, object storage, or
another large-file endpoint before they can be used as direct fallbacks.

## OpenNeuro ds000102 Flanker Subset

Original source:

- OpenNeuro dataset page: `https://openneuro.org/datasets/ds000102`
- GitHub mirror: `https://github.com/OpenNeuroDatasets/ds000102`
- S3 source used by the tutorial plugin:
  `https://s3.amazonaws.com/openneuro.org/ds000102`

Credit:

- Dataset metadata reports accession `ds000102` from the OpenfMRI database.
- The bundled `dataset_description.json` lists the authors and publication
  references. Keep that file with any redistribution.
- License in `dataset_description.json`: `PDDL`.

Mirrored files:

- `dataset_description.json`
- `participants.tsv`
- `T1w.json`
- `task-flanker_bold.json`
- `sub-01/anat/sub-01_T1w.nii.gz`
- `sub-01/func/sub-01_task-flanker_run-1_bold.nii.gz`
- `sub-01/func/sub-01_task-flanker_run-1_events.tsv`

Tutorial use:

- Used by the BIDS/NIfTI walkthrough.
- Imported into XNAT as a project-level `BIDS` resource.
- The subset is intentionally one subject and one functional run so workshop
  imports finish quickly.

## TCIA QIN-PROSTATE-Repeatability DICOM Subset

Original source:

- TCIA collection page:
  `https://www.cancerimagingarchive.net/collection/qin-prostate-repeatability/`
- NBIA API base used by the tutorial plugin:
  `https://services.cancerimagingarchive.net/nbia-api/services/v4`
- SeriesInstanceUID:
  `1.3.6.1.4.1.14519.5.2.1.3671.4754.216645718571142540899096273555`

Credit:

- Data are from The Cancer Imaging Archive (TCIA)
  QIN-PROSTATE-Repeatability collection.
- License label used by the tutorial manifest:
  Creative Commons Attribution 4.0 International.
- The NBIA ZIP includes a `LICENSE` file; keep it inside the archive.

Mirrored files:

- `datasets/xnat-tutorial/tcia_dicom_intro/archive.zip`
- `datasets/xnat-tutorial/tcia_collection_smallest/archive.zip`

The two ZIP files are the same tiny series. The second path is present so the
plugin's collection-mode fallback has a stable archive URL.

Tutorial use:

- Used by DICOM basics, general XNAT usage, and viewer/resource walkthroughs.
- Routed through XNAT's prearchive with auto-archive enabled.

## TCIA Mouse-Astrocytoma DICOM Subset

Original source:

- TCIA collection page:
  `https://www.cancerimagingarchive.net/collection/mouse-astrocytoma/`
- NBIA API base used by the tutorial plugin:
  `https://services.cancerimagingarchive.net/nbia-api/services/v4`
- SeriesInstanceUID:
  `1.3.46.670589.11.17169.5.0.5780.2010092712094570350.3`

Credit:

- Data are from The Cancer Imaging Archive (TCIA) Mouse-Astrocytoma
  collection.
- License label used by the tutorial manifest:
  Creative Commons Attribution 3.0 Unported.
- The NBIA ZIP includes a `LICENSE` file; keep it inside the archive.

Mirrored files:

- `datasets/xnat-tutorial/tcia_mouse_astrocytoma_mri/archive.zip`

Tutorial use:

- Used as a tiny preclinical MRI option for XNAT import practice.
- Routed through XNAT's prearchive with auto-archive enabled.

## Medical Segmentation Decathlon Task04 Hippocampus

Original source:

- MONAI MSD mirror:
  `https://msd-for-monai.s3.us-west-2.amazonaws.com/Task04_Hippocampus.tar`
- Medical Segmentation Decathlon:
  `http://medicaldecathlon.com/`

Credit:

- Data are from the Medical Segmentation Decathlon Task04 Hippocampus
  challenge dataset.
- License label used by the tutorial importer: Creative Commons
  Attribution-ShareAlike 4.0 International.

Mirrored files:

- `datasets/grouplevel/msd/Task04_Hippocampus.tar`

Tutorial use:

- Used by the nnU-Net dataset builder walkthrough through the Group-Level
  tutorial importer.
- The default tutorial import uses a smoke subset of up to 10 training cases;
  users can request the full set from the downloader UI.

## Group-Level Source Catalog

`datasets/grouplevel/sources.yml` lists every public source exposed by the
Group-Level tutorial import UI: OpenNeuro ds000102 Flanker, the MSD tasks, and
the preclinical TumSeg micro-CT dataset. The catalog records source URL,
expected size, license label, and whether a raw GitHub fallback is present.

Large entries are intentionally cataloged but not mirrored as normal Git files
because GitHub rejects individual files over 100 MB and raw repository storage
is not appropriate for multi-GB archives. If those need first-class fallbacks,
publish them as release assets or in object storage and configure the importer
source URL properties to those endpoints.

## Checksums

`SHA256SUMS` records the checksum for every mirrored file.

Verify with:

```bash
shasum -a 256 -c SHA256SUMS
```

## How Plugins Use This Repository

The tutorial plugin has a default fallback base URL:

```properties
xnat.tutorial.datasets.fallbackBaseUrl=https://raw.githubusercontent.com/xnatworks/xnat_tutorial_datasets/main
```

The bundled tutorial manifest also pins explicit fallback URLs:

```yaml
fallback_https_prefix: https://raw.githubusercontent.com/xnatworks/xnat_tutorial_datasets/main/datasets/openneuro/ds000102
fallback_archive_url: https://raw.githubusercontent.com/xnatworks/xnat_tutorial_datasets/main/datasets/xnat-tutorial/tcia_dicom_intro/archive.zip
```

Use normal Git file storage for these assets. Do not replace them with Git LFS
pointers unless the plugin is changed to download through GitHub release assets
or another LFS-aware endpoint.

## Adding or Refreshing Data

1. Download from the original public source.
2. Preserve upstream metadata and license files.
3. Keep the tutorial subset as small as possible.
4. Update `SHA256SUMS`.
5. Update this README with source, credit, license, and exact mirrored files.
6. Update the tutorial plugin manifest if the fallback path changes.
