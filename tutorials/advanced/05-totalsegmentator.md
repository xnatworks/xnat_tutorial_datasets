# TotalSegmentator

**Level:** Advanced · **Time:** 30–90 s per scan ·
**Recommended dataset:** [`tcia_prostate_aec`](../datasets/tcia_prostate_aec.md)
(or any full-FOV abdomen+pelvis CT, e.g. a TCIA CPTAC-SAR venous
ABD/PELVIS series)

Run the community-standard **TotalSegmentator** container on a CT scan
and inspect the multi-label segmentation it writes back to XNAT. The
goal is to teach the deployment pattern (containerised inference,
output as a scan-level resource) on a model that just works
out-of-the-box — no training, no dataset prep.

| | |
|---|---|
| Image | `wasserth/totalsegmentator` (or your site's pinned fork) |
| Architecture | nnU-Net ensemble |
| Output labels | 117 |
| Walltime / abdomen CT | 30–90 s |
| GPU | ≥ 8 GiB VRAM |

## Preconditions

- Container Service installed.
- A `totalsegmentator-scan` (or equivalent) wrapper enabled at site and
  project scope. The pinned image and exact wrapper id depend on your
  site — see [admin/02 command enablement](../admin/02-command-enablement.md).
- A CT scan with a real abdomen+pelvis field of view. The
  `tcia_dicom_intro` series is too small to produce meaningful
  segmentation output; use it only to demo the *workflow*, not the
  biological result.

## Walkthrough

### 1. Run TotalSegmentator on a scan

1. Open a session containing a usable CT scan.
2. Right-click the scan → **Run Container** → `totalsegmentator-scan`.
3. Choose the **output type**:
   - `dicom` — RTStruct DICOM (writes the segmentation as a DICOM
     structure set; OHIF Viewer can render this directly as a contour
     overlay).
   - `nifti` — multi-label NIfTI (one volume; easier to script against,
     but needs Workbench to view as an overlay).
4. Accept the other defaults; click **Run**.
5. Open command history and wait for the run to reach `Complete`. On a
   fresh GPU node the first run takes several minutes (image pull),
   then 30–90 s of inference.
6. Return to the scan. Find the new output resources:
   - `TotalSegmentator-Output` — the RTStruct DICOM or multi-label
     NIfTI, depending on output type.
   - `TotalSegmentator-Stats` — `statistics.json` (per-organ volume,
     intensity stats).
   - `TotalSegmentator-Radiomics` — `statistics_radiomics.json`
     (optional, populated for some wrapper variants).

REST equivalent in
[reference/rest-cheatsheet § Container launches](../reference/rest-cheatsheet.md#container-launches).
Wrapper id and command id are site-specific — pull them with
`GET /xapi/commands` first.

### 2. View the RTStruct overlay in OHIF

When `output-type = dicom`, the output is an **RTStruct DICOM** that
OHIF can render as a contour overlay on top of the source CT. On a
site that promotes RTStruct outputs to ROI Collection assessors (the
demo XNAT does), the assessor shows up directly in OHIF's contours
panel — no manual import required.

Reference: [Using the XNAT OHIF Viewer](https://wiki.xnat.org/xnat-ohif-viewer/using-the-xnat-ohif-viewer).

1. Open the session report page.
2. In the **Actions** box, click **View Images** to open OHIF in the
   active window. (Middle-click or right-click to open in a new
   tab/window.)
3. The source CT loads first. Scroll through to confirm you are on the
   intended scan.
4. Open the **Contours Panel** (the ROI / region-of-interest panel on
   the right of the viewport).
5. The TotalSegmentator ROI Collection should already be listed
   (label like `TS-RTStruct-PC-<session>--scan-<n>-<timestamp>`). If
   it is not, click **Import** and pick it from the available
   collections — RTStruct outputs that did not auto-promote can still
   be imported manually.
6. Imported contours arrive **locked** (viewable but not editable).
   Click each ROI name in the panel to:
   - Highlight the contour in the viewport.
   - Show its slice count.
   - Click the slice count to jump to the slice nearest the ROI
     centre.
7. Use the eye / visibility toggle on each ROI to compare individual
   organs against the source CT.
8. (Optional) Toggle **Stats** in the panel to overlay ROI statistics
   on the viewport.

### 3. View the NIfTI segmentation in Workbench

When `output-type = nifti`, the output is a multi-label NIfTI rather
than RTStruct. OHIF cannot open NIfTI files. Use **Open with
Workbench** on the source CT and load the segmentation as an overlay —
see [intro/09 — Open with Workbench](../intro/09-niivue-overlays.md).

## Expected output

For RTStruct output (`output-type = dicom`):

- An RTStruct DICOM file under the `TotalSegmentator-Output` scan
  resource (one file, typically a few MB).
- A session-level ROI Collection assessor (`icr:roiCollectionData`)
  with a label like `TS-RTStruct-PC-<session>--scan-<n>-<timestamp>`,
  populated when the site has RTStruct → ROI Collection promotion
  configured (the demo XNAT does; check
  [admin/02 command enablement](../admin/02-command-enablement.md) if
  yours does not).
- One ROI per anatomical structure TotalSegmentator recognised in the
  field of view (typically 50–80 distinct ROIs on a full abdomen+pelvis
  CT).
- Optional resources (when the wrapper variant produces them):
  `TotalSegmentator-Stats` / `statistics.json` with per-organ volume
  and intensity summaries; `TotalSegmentator-Radiomics` /
  `statistics_radiomics.json`.

For NIfTI output (`output-type = nifti`):

```python
import nibabel as nib, numpy as np
n = nib.load("segmentations.nii.gz").get_fdata().astype(int)
labels = np.unique(n)
print(f"shape: {n.shape}")
print(f"labels: {len(labels)}")
print(f"non-zero voxels: {int((n > 0).sum())}")
```

For a full abdomen+pelvis CT, expect 70–80 distinct labels populated
and several million non-zero voxels.

## Verify

For RTStruct: open OHIF, load the imported ROI collection, and
confirm large organs (liver, spleen, kidneys) sit on the expected
anatomy when you scroll through slices.

For NIfTI: the output volume loads with the same affine as the source
CT in Workbench; the same large organs appear in the expected
positions when overlaid.

## If it does not work

Open command history → failed run → stderr.

- **CUDA / memory errors**: environment issue. The full TotalSegmentator
  ensemble needs ≥ 8 GiB VRAM. If your GPU is smaller, look for a
  lowres-mode variant in your wrapper config.
- **`pending` or `Failed (Rejected)` after a long wait**: a Swarm
  constraint mismatch. Read command history → `rejected` event for
  the exact reason. Two common ones on a 16 GB / 4 vCPU GPU node
  (`g4dn.xlarge`):
  - `reserve-memory` / `limit-memory` exceed node RAM. Drop them to
    `14000` / `15000` (MB) to leave OS headroom.
  - `limit-cpu` exceeds available vCPUs. `g4dn.xlarge` has 4 vCPUs;
    set `limit-cpu` to ≤ 4.
- **Empty mask**: the input field of view does not contain enough of
  the expected anatomy. Use a real abdomen+pelvis CT, not a
  localiser-style series.
- **Wrong scan got mounted**: launch context error. Always launch from
  the scan you intend to segment.
- **OHIF cannot find the RTStruct**: confirm the wrapper wrote DICOM,
  not NIfTI. OHIF only renders the DICOM RTStruct path.

## Talking points

- **TotalSegmentator** is the community-blessed gold standard for
  whole-body CT segmentation, built on nnU-Net ensembles trained on a
  large multi-centre cohort.
- Inference output is plain NIfTI — easy to consume with viewers,
  downstream containers, or analysis scripts.
- For training your own segmentation model on a paired image+label
  dataset, see [advanced/02 nnU-Net dataset](02-nnunet-dataset.md).

## Reference

- [TotalSegmentator on GitHub](https://github.com/wasserth/TotalSegmentator).
- [Wasserthal et al., *Radiology AI* 2023](https://pubs.rsna.org/doi/10.1148/ryai.230024) —
  the TotalSegmentator paper.
