# XNAT Tutorials

Walkthrough tutorials that pair XNAT features, containers, and downloadable
sample datasets from this repository. Self-paced and instructor-led use
both supported.

## Container walkthroughs

| # | Tutorial | Category | Dataset / container | Walltime |
|---|---|---|---|---|
| 01 | [DICOM → NIfTI conversion](01-dcm2niix.md) | Container basics | `tcia_dicom_intro` + `xnatworks/dcm2niix:2.0` | < 1 min |
| 02 | [MONAI Bundle segmentation](02-monai-bundle-segmentation.md) | AI segmentation | CT abdomen/pelvis + `xnatworks/monai-bundle-nifti:0.3.0` | 5–15 s per scan |
| 03 | [TotalSegmentator vs MONAI comparison](03-totalsegmentator-vs-monai.md) | AI comparison | same CT as 02 + TotalSegmentator/MONAI | ~5 min total |
| 04 | [RABIES — rodent fMRI preprocess](04-rabies-rodent-fmri.md) | Preclinical BIDS | OpenNeuro `ds002551` + `ghcr.io/cobralab/rabies:0.6.0` | 10–60 min |
| 05 | [Dynamic Data Type and Custom Form](05-dynamic-data-type.md) | XNAT administration | `tcia_dicom_intro` sample project | 10–15 min |
| 06 | [Preclinical Metadata as a Dynamic Data Type](06-preclinical-metadata-datatype.md) | Metadata / preclinical | OpenNeuro `ds002551` `participants.tsv` | 10–15 min |

## Dataset walkthroughs

One per dataset shipped by the
[xnat_tutorial_plugin](https://github.com/xnatworks/xnat_tutorial_plugin)
manifest. Focuses on **why the data matters** first, XNAT plumbing
second. See [`datasets/README.md`](datasets/README.md) for the index and
cross-cutting tutorial arcs.

Per-dataset upstream sources, licenses, and local-mirror fallbacks are
listed in [`sources.yml`](sources.yml).

Each tutorial is self-contained — pre-reqs, dataset download when needed,
step-by-step launch or admin workflow, expected outputs, and what to inspect.

## Running on any XNAT

Container tutorials assume an XNAT instance with Container Service installed
and Docker access for the running user. Admin/UI tutorials require the named
XNAT feature and a site administrator account. REST examples reference
`${XNAT_HOST}`, `${XNAT_USER}`, `${XNAT_PASS}`, `${PROJECT}`, `${SESSION}`,
`${SCAN}`, and `${WRAPPER_ID}` as placeholders — set them in your shell or
substitute inline.

## Common preconditions

1. XNAT 1.10 or newer for the full workshop path.
2. Tutorial plugin installed when you want one-click sample data loading.
3. Container Service plugin enabled for container exercises.
4. Docker host with GPU support where needed (CUDA 12+ recommended for
   MONAI/RABIES).
5. The relevant container command registered + enabled on the project. Each
   container tutorial shows the registration `curl` for its container.

## Dataset download

Datasets in this repo are course-sized subsets. Tutorials reference them
via:

```
https://raw.githubusercontent.com/xnatworks/xnat_tutorial_datasets/main/datasets/<path>
```

Larger source datasets are linked to their original homes (TCIA, OpenNeuro,
etc.) — also documented in the [main README](../README.md).

## Tutorial conventions

- Code blocks use `bash` — copy-paste runnable
- `${XNAT_HOST}` and `${XNAT_USER}:${XNAT_PASS}` are placeholders — set them
  in your shell first
- Screenshots / GIFs in `images/` per tutorial directory (added later)
- Each tutorial ends with **Expected output** + **Verify** sections so a
  learner can confirm a successful run

## Maintenance

Tutorials live under `tutorials/`. Datasets they reference are mirrored
under `datasets/` (raw GitHub) and pulled live from upstream when
available. The plugin manifest at `tutorials/sources.yml` is the
authoritative dataset source list.
