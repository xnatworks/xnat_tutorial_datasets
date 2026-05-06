# MONAI Bundle segmentation

**Level:** Advanced · **Time:** 5–15 s per scan once running ·
**Recommended dataset:** a full-FOV abdomen+pelvis CT
(e.g. TCIA CPTAC-SAR_v9 venous ABD/PELVIS)

A single XNAT container that runs **any** pre-baked MONAI Bundle on either
a DICOM scan or a NIfTI resource, with the model selected at launch time
from a UI dropdown.

| | |
|---|---|
| Image | `xnatworks/monai-bundle-nifti:0.3.0` |
| Source repo | https://github.com/xnatworks/xnat_monailabel_container |
| Pre-baked bundles (default) | `spleen_ct_segmentation`, `wholeBody_ct_segmentation` |
| Wrapper contexts | scan (DICOM auto-converted), session pick-scan, session-resource (NIfTI) |
| GPU | required (CUDA 12+, ≥ 8 GiB VRAM) |

## Why this container

- **Parameterised model**: same container, dropdown picks the bundle.
- **DICOM auto-conversion**: if the input mount has DICOMs but no NIfTI,
  the container runs dcm2niix internally.
- **Anonymisation cleanup fallback**: if dcm2niix rejects a series due to
  bad private tags or `IconImageSequence`, the container retries after
  stripping them via pydicom.
- **wholeBody lowres mode**: highres needs ~18 GiB VRAM; the container
  auto-overrides to lowres (3 mm spacing, `model_lowres.pt`) to fit on a
  16 GiB card.

## Dataset note

`tcia_dicom_intro` has too small a field of view for spleen / wholeBody
to produce a non-empty output. Use it only to demo the *workflow*, not
the biological result. For real model output, use a full abdomen+pelvis
CT.

## Walkthrough

1. Open a session. Right-click a scan → **Run Container** →
   **MONAI Bundle Segmentation (DICOM scan)**.
2. Pick the **Model** from the dropdown:
   - `spleen_ct_segmentation` — binary spleen mask.
   - `wholeBody_ct_segmentation` — up to 105 labels.
3. Click **Run**. Open command history.
4. When `Complete`, return to the scan. Open the new
   `MONAI_SEG_NIFTI` resource.

REST equivalent in
[reference/rest-cheatsheet](../reference/rest-cheatsheet.md#container-launches).
The wrapper id is `monai-bundle-seg-on-scan` under the
`xnatworks/monai-bundle-nifti` command.

## Expected output

| Model | Output shape | Unique labels |
|---|---|---|
| `spleen_ct_segmentation` | matches input | `{0, 1}` |
| `wholeBody_ct_segmentation` | matches input | up to 105 |

## Verify

```python
import nibabel as nib, numpy as np
n = nib.load("segmentation.nii.gz")
d = n.get_fdata()
u = np.unique(d.astype(int))
print(f"shape: {d.shape}")
print(f"labels: {len(u)}")
print(f"non-zero voxels: {int((d > 0).sum())}")
```

For a full abdomen+pelvis CT with wholeBody, expect 70–80 unique labels
and millions of non-zero voxels.

## If it does not work

Open command history → failed run → stderr.

- **CUDA / memory errors**: environment issue; use a smaller model or a
  bigger card. wholeBody lowres should fit on 16 GiB.
- **Empty mask**: the input field of view does not contain the target
  anatomy. Use a full abdomen+pelvis CT, not a localiser-style series.
- **Conversion error**: the container's anonymisation cleanup retry
  should kick in automatically. If it fails again, inspect the source
  DICOM — private tags may be more than the cleanup expects.

## Adding more bundles

Extend `PREBAKED_BUNDLES` in the
[`nifti-to-nifti/Dockerfile`](https://github.com/xnatworks/xnat_monailabel_container/blob/main/nifti-to-nifti/Dockerfile)
and rebuild:

```dockerfile
ARG PREBAKED_BUNDLES="spleen_ct_segmentation:0.5.7 wholeBody_ct_segmentation:0.2.0 brats_mri_segmentation:0.4.9"
```

Suggested expansions:

- `brats_mri_segmentation` — brain tumour (T1, T1c, T2, FLAIR).
- `lung_nodule_ct_detection`.
- `pancreas_ct_dints_segmentation`.
- `swin_unetr_btcv_segmentation` — 13-organ abdomen, transformer-based.

## Talking points

- One container, many models — that is the deployment story for AI
  inference.
- Model agility comes from the MONAI Bundle format (self-contained
  config + weights + transforms).
- Output is plain NIfTI — pair with
  [advanced/03-segmentation-comparison](03-segmentation-comparison.md)
  to compare against TotalSegmentator.
