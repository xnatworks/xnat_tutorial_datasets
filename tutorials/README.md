# XNAT Container Tutorials

Walkthrough tutorials that pair each demo container with a downloadable
sample dataset from this repository. Designed for live workshops
(foundingGIDE 2026, Heidelberg) and self-paced learning.

## Container walkthroughs

| # | Tutorial | Container | Walltime |
|---|---|---|---|
| 01 | [DICOM → NIfTI conversion](01-dcm2niix.md) | `xnatworks/dcm2niix:2.0` | < 1 min |
| 02 | [MONAI Bundle segmentation](02-monai-bundle-segmentation.md) | `xnatworks/monai-bundle-nifti:0.3.0` | 5–15 s per scan |
| 03 | [TotalSegmentator vs MONAI comparison](03-totalsegmentator-vs-monai.md) | `wasserth/totalsegmentator` + `xnatworks/monai-bundle-nifti` | ~5 min total |
| 04 | [RABIES — rodent fMRI preprocess](04-rabies-rodent-fmri.md) | `ghcr.io/cobralab/rabies:0.6.0` | 10–60 min |

## Dataset walkthroughs

One per dataset shipped by the
[xnat_tutorial_plugin](https://github.com/xnatworks/xnat_tutorial_plugin)
manifest. Focuses on **why the data matters** first, XNAT plumbing
second. See [`datasets/README.md`](datasets/README.md) for the index and
cross-cutting workshop arcs.

Per-dataset upstream sources, licenses, and local-mirror fallbacks are
listed in [`sources.yml`](sources.yml).

Each tutorial is self-contained — pre-reqs, dataset download, step-by-step
launch via XNAT UI **and** REST, expected outputs, what to inspect.

## Running on any XNAT

All tutorials assume an XNAT instance with Container Service installed and
Docker access for the running user. They reference `${XNAT_HOST}`,
`${XNAT_USER}`, `${XNAT_PASS}`, `${PROJECT}`, `${SESSION}`, `${SCAN}`, and
`${WRAPPER_ID}` as placeholders — set them in your shell or substitute
inline.

## Common preconditions

1. Container Service plugin enabled
2. Docker host with GPU support (CUDA 12+ recommended for MONAI/RABIES)
3. The relevant container command registered + enabled on the project. Each
   tutorial shows the registration `curl` for its container.

## Dataset download

Datasets in this repo are workshop-sized subsets. Tutorials reference them
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

This branch (`tutorials/foundinggide-2026`) is the working branch for the
May 2026 workshop materials. Stable additions get rebased onto `main`
afterwards.
