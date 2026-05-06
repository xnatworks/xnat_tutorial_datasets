# TotalSegmentator vs MONAI

**Level:** Advanced · **Time:** ~5 min total ·
**Recommended dataset:** the same abdomen+pelvis CT used in
[advanced/02-monai-segmentation](02-monai-segmentation.md)

Run two whole-body segmentation pipelines on the **same** scan and
compare. The point is to teach learners that container output is not
automatically correct — comparison on a known input is how you tell which
tool to trust on which structures.

| | TotalSegmentator | MONAI wholeBody |
|---|---|---|
| Image | `wasserth/totalsegmentator` (or fork) | `xnatworks/monai-bundle-nifti:0.3.0` |
| Architecture | nnU-Net ensemble | SegResNet (single network) |
| Output labels | 117 | up to 105 (TotalSegmentator-derived) |
| Walltime / abdomen CT | 30–90 s | 6–15 s (lowres) |
| GPU | ≥ 8 GiB | ≥ 8 GiB lowres / 16 GiB+ highres |

## Walkthrough

### 1. Run TotalSegmentator

Right-click the scan → **Run Container** → `totalsegmentator-scan`. Wait
for `Complete`. Output is typically RTSTRUCT (DICOM) plus a multi-label
NIfTI.

### 2. Run MONAI wholeBody

Right-click the same scan → **Run Container** →
**MONAI Bundle Segmentation (DICOM scan)** → Model =
`wholeBody_ct_segmentation`. Wait for `Complete`. Output is a single
multi-label NIfTI in `MONAI_SEG_NIFTI`.

### 3. Pull both outputs

```bash
curl -O ${XNAT_HOST}/data/projects/${PROJECT}/experiments/${SESSION}/scans/${SCAN}/resources/SEG_NIFTI/files/segmentations.nii.gz
curl -O ${XNAT_HOST}/data/projects/${PROJECT}/experiments/${SESSION}/scans/${SCAN}/resources/MONAI_SEG_NIFTI/files/segmentation.nii.gz
```

### 4. Compare

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

Open both files plus the source CT in the same viewer (3D Slicer,
ITK-SNAP, NiiVue).

## Verify

Both NIfTIs load with the same affine as the source CT. Large organs
(liver, spleen, kidneys) overlap heavily; small structures (vessels,
glands) diverge more.

## If it does not work

- **One file missing or empty**: open command history for the failed run
  and read stderr first. Empty masks usually mean the field of view does
  not include the target anatomy.
- **Outputs attached to the wrong scan**: a launch context mistake — both
  containers must launch from the same source scan.

## Talking points

- **TotalSegmentator** is the community-blessed gold standard, built on
  nnU-Net ensembles.
- **MONAI Bundle** is single-network SegResNet — lighter, faster, slightly
  fewer labels, same training-data lineage.
- Tradeoff: TotalSegmentator's ensemble is more robust on hard cases;
  MONAI bundle is ~10× faster and friendlier for interactive demos and
  many-scan batch jobs.
- Discussion prompt: which structures would you trust each tool on, and
  what would change that answer?
