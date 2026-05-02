# Tutorial 04 — MONAI Bundle segmentation (parameterized model)

A single XNAT container that runs **any** pre-baked MONAI Bundle on either
a DICOM scan or a NIfTI resource, with the model selected at launch time
via a UI dropdown.

## Container

| Field | Value |
|---|---|
| Image | `xnatworks/monai-bundle-nifti:0.3.0` |
| Source repo | https://github.com/xnatworks/xnat_monailabel_container |
| Pre-baked bundles (default) | `spleen_ct_segmentation`, `wholeBody_ct_segmentation` |
| Wrapper contexts | scan (DICOM auto-converted), session+pick-scan, session-resource (NIfTI) |
| Walltime | spleen: 2–10 s, wholeBody lowres: 6–15 s |
| GPU | required (CUDA 12+, ≥ 8 GiB VRAM) |

## Features worth pointing out

- **Parameterized model** — same container; dropdown picks the bundle.
- **DICOM auto-conversion** — if the input mount has DICOMs and no NIfTI
  sibling, the container runs `dcm2niix` internally.
- **Anonymization-cleanup fallback** — if dcm2niix rejects the series due
  to bad private tags / IconImageSequence, the container retries after
  stripping them via pydicom.
- **wholeBody lowres mode** — `wholeBody_ct_segmentation` defaults to
  highres which needs ~18 GiB VRAM; the container auto-overrides to
  lowres (3 mm spacing, model_lowres.pt) to fit on a 16 GiB card.

## Dataset

See [`sources.yml`](sources.yml) entry `04-monai-bundle-segmentation`.
Recommended: a full-FOV abdomen+pelvis CT (e.g. TCIA-CPTAC-SAR_v9 venous
ABD/PELVIS) so both `spleen` and `wholeBody` produce non-empty output.

The minimal `tcia_dicom_intro` from Tutorial 01 has too small a FOV for
spleen/wholeBody — output will be empty. Use it only to demo the
*workflow*, not the biological result.

## Beginner Guidance

What this tutorial does: runs an AI segmentation container on a scan or NIfTI
resource and writes a mask back to XNAT as a derived resource. The source image
stays unchanged.

What to look for before launch: use a CT scan with enough anatomy for the
selected model. The launch dialog should show the scan input and a model
dropdown. If the model expects CT abdomen/pelvis, a small localizer or tiny
field-of-view scan is not a good biological example.

How to know it worked: the run reaches `Complete` and the scan has a
`MONAI_SEG_NIFTI` resource containing a segmentation `*.nii.gz`. For a real
abdomen/pelvis example, the mask should contain non-zero labels.

What to check first if it fails: open command history and read stderr for CUDA,
memory, model name, or image-conversion errors. Then verify the project has GPU
capacity and the input scan/resource is the image you intended to segment.

## Run via UI

1. Navigate to a session.
2. Right-click a scan → **Run Container** → **MONAI Bundle Segmentation (DICOM scan)**.
3. Pick the **Model** from the dropdown — `spleen_ct_segmentation` or
   `wholeBody_ct_segmentation`.
4. Click **Run**.
5. Open command history and wait for `Complete`.
6. Return to the scan and open `MONAI_SEG_NIFTI`.

## Run via REST

```bash
SESS=$(curl -s -u ${XNAT_USER}:${XNAT_PASS} ${XNAT_HOST}/data/JSESSION)

curl -b "JSESSIONID=$SESS" -X POST -H 'Content-Type: application/json' \
  -d '{
        "scan":"/archive/projects/${PROJECT}/experiments/${SESSION}/scans/${SCAN}",
        "model_name":"wholeBody_ct_segmentation"
      }' \
  ${XNAT_HOST}/xapi/projects/${PROJECT}/wrappers/${WRAPPER_ID}/launch
```

Resolve `${WRAPPER_ID}` from `GET /xapi/commands` (command name
`xnatworks/monai-bundle-nifti`, wrapper `monai-bundle-seg-on-scan`).

## Expected output

A scan-level resource `MONAI_SEG_NIFTI` containing one `*.nii.gz`:

| Model | Output shape | Unique labels | Notes |
|---|---|---|---|
| `spleen_ct_segmentation` | matches input | `{0, 1}` | binary spleen mask |
| `wholeBody_ct_segmentation` | matches input | up to 105 | TotalSegmentator-equivalent label space |

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

## Adding more bundles

Extend `PREBAKED_BUNDLES` in the source repo's
[`nifti-to-nifti/Dockerfile`](https://github.com/xnatworks/xnat_monailabel_container/blob/main/nifti-to-nifti/Dockerfile)
and rebuild. New options appear automatically in the dropdown.

```dockerfile
ARG PREBAKED_BUNDLES="spleen_ct_segmentation:0.5.7 wholeBody_ct_segmentation:0.2.0 brats_mri_segmentation:0.4.9"
```

Suggested expansions:

- `brats_mri_segmentation` — brain tumor (T1, T1c, T2, FLAIR)
- `lung_nodule_ct_detection` — lung nodule detection
- `pancreas_ct_dints_segmentation` — pancreas, NAS architecture
- `swin_unetr_btcv_segmentation` — 13-organ abdomen, transformer-based

## Talking points

- One container, many models — the deployment story for AI inference.
- Model agility comes from the MONAI Bundle format (self-contained
  config + weights + transforms).
- DICOM-or-NIfTI flexibility lets the same container slot into different
  points in a workflow.
- Output is plain NIfTI — pair with [Tutorial 05](05-totalsegmentator-vs-monai.md)
  to compare against TotalSegmentator.
