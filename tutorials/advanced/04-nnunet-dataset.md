# nnU-Net dataset builder (MSD Hippocampus)

**Level:** Advanced · **Time:** 30–60 min ·
**Recommended dataset:** `msd_hippocampus_nnunet`

Use the Group-Level tutorial loader to build an nnU-Net-shaped dataset
from the MSD Task04 Hippocampus archive, and connect that dataset back
to XNAT subjects, scans, and label resources.

## Container / loader

| | |
|---|---|
| Source archive | `datasets/grouplevel/msd/Task04_Hippocampus.tar` |
| Loader | Group-Level tutorial dataset import UI |
| Default smoke subset | up to 10 training cases |

## Walkthrough

1. Open the Group-Level tutorial loader (Admin → **Group-Level Datasets**
   if installed; otherwise the loader entry in
   **Tools → XNAT Tutorial & Walkthrough**).
2. Pick **MSD Task04 Hippocampus**.
3. Pick the smoke subset (default) for tutorials. Larger subsets are
   for batch demos.
4. Run the import. Watch the progress per case.
5. When done, open the project. Confirm subjects, sessions, and scan
   resources representing each MSD case.
6. Open one subject. Find the source NIfTI and the label NIfTI as
   **separate** scan resources or session resources.
7. Open both in a viewer that supports overlays. Toggle the label.

## Expected result

- A project populated with one subject per MSD case.
- Each case has a source T1w-style NIfTI and a paired label volume.
- Provenance is captured at project level (source URL, MSD task ID,
  license).

## Verify

You can find the MSD case ID, the source image, and the label volume for
the same subject without ambiguity.

## If it does not work

- **Loader stalls on first run**: the MSD tar is several hundred MB.
  Confirm the loader has network access to the GitHub raw URL or to the
  MONAI mirror in `datasets/grouplevel/sources.yml`.
- **Labels open as separate scans rather than overlays**: that is normal.
  Overlay rendering is a viewer setting, not an XNAT structure.

## Run nnU-Net inference

Once the project is built, the same scans can serve as input to an
nnU-Net inference container. The XNAT-side responsibility is providing
the right NIfTI in the right resource label; the inference container's
configuration is pipeline-specific and outside this lesson's scope.

## Talking points

- "Group-level" datasets in XNAT live alongside per-session datasets.
  The loader maps each case to the standard hierarchy.
- For hippocampus segmentation, MSD Task04 has matched source + ground
  truth — useful for building training sets and for evaluating
  pretrained models on a known-truth dataset.
- nnU-Net's plan/preprocess pipeline expects a specific dataset folder
  layout. If you push past the smoke subset, plan disk and runtime
  budgets accordingly.

## Reference

- [Medical Segmentation Decathlon](http://medicaldecathlon.com/)
- `datasets/grouplevel/sources.yml` — full source catalog including
  larger MSD tasks and the preclinical TumSeg micro-CT dataset.
