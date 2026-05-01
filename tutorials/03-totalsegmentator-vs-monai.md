# Tutorial 03 — TotalSegmentator vs MONAI wholeBody (comparison)

Run two whole-body segmentation pipelines on the **same** abdomen CT and
compare. Discuss agreement, label-space differences, and runtime tradeoffs.

## Containers

| | TotalSegmentator | MONAI wholeBody |
|---|---|---|
| Image | `wasserth/totalsegmentator` (or fork) | `xnatworks/monai-bundle-nifti:0.3.0` |
| Architecture | nnU-Net ensemble | SegResNet (single network) |
| Output labels | 117 | up to 105 (TotalSegmentator-derived) |
| Walltime on 1 abdomen CT | 30–90 s | 6–15 s (lowres mode) |
| GPU | ≥ 8 GiB | ≥ 8 GiB (lowres) / 16 GiB+ (highres) |

## Dataset

See [`sources.yml`](sources.yml) entry `03-totalsegmentator-vs-monai`
(aliased to `02-monai-bundle-segmentation`). Use the same abdomen+pelvis
CT for both pipelines.

## Run

### TotalSegmentator

Right-click the scan → **Run Container** → `totalsegmentator-scan`.
Output is typically RTSTRUCT (DICOM) plus a multi-label NIfTI.

### MONAI wholeBody

Right-click the scan → **Run Container** → **MONAI Bundle Segmentation (DICOM scan)**
→ Model = `wholeBody_ct_segmentation`. Output is a single multi-label
NIfTI as a `MONAI_SEG_NIFTI` resource.

## Compare

Pull both segmentations down and load into a viewer (3D Slicer, ITK-SNAP,
NiiVue):

```bash
# TotalSegmentator NIfTI variant
curl -O ${XNAT_HOST}/data/projects/${PROJECT}/experiments/${SESSION}/scans/${SCAN}/resources/SEG_NIFTI/files/segmentations.nii.gz

# MONAI output
curl -O ${XNAT_HOST}/data/projects/${PROJECT}/experiments/${SESSION}/scans/${SCAN}/resources/MONAI_SEG_NIFTI/files/segmentation.nii.gz
```

Quick stats:

```python
import nibabel as nib, numpy as np
ts = nib.load("segmentations.nii.gz").get_fdata().astype(int)
mn = nib.load("segmentation.nii.gz").get_fdata().astype(int)

print("TS labels:", np.unique(ts).size, " voxels:", int((ts > 0).sum()))
print("MN labels:", np.unique(mn).size, " voxels:", int((mn > 0).sum()))

for l in sorted(set(np.unique(ts)) | set(np.unique(mn))):
    if l == 0: continue
    print(f"label {l:3d}: TS={int((ts==l).sum()):>10d}  MN={int((mn==l).sum()):>10d}")
```

## Talking points

- **Reference pipeline (TotalSegmentator)** — community-blessed gold
  standard built on nnU-Net ensembles.
- **MONAI bundle** — single-network SegResNet, lighter, faster, slightly
  fewer labels, same training-data lineage.
- Agreement on large organs (liver, spleen, kidneys) is typically very
  high; small structures (vessels, glands) diverge more.
- Tradeoff: TotalSegmentator's ensemble is more robust; MONAI bundle is
  ~10× faster and friendlier for interactive demos / many-scan batch
  jobs.

## What's next

For brain segmentation comparison, see
[Tutorial 04 — RABIES rodent fMRI](04-rabies-rodent-fmri.md), which steps
into the truly preclinical (small-animal) territory.
