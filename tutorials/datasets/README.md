# Dataset walkthroughs

One walkthrough per dataset shipped by the [XNAT tutorial
plugin](https://github.com/xnatworks/xnat_tutorial_plugin). Each focuses
on **why the data matters** first, **how XNAT handles it** second.

The plugin manifest (`config/tutorial-datasets.example.yml`) defines:
upstream sources, fallback mirrors in this repo, ingest mechanics
(NBIA REST, S3, archive zip, group-level loader), and project defaults.
These walkthroughs are the human-facing companion.

## Datasets

| Dataset | Modality | Format | Walkthrough |
|---|---|---|---|
| TCIA QIN-PROSTATE-Repeatability MR series | MR | DICOM | [tcia_dicom_intro](tcia_dicom_intro.md) |
| TCIA Pseudo-PHI-DICOM-Data | CT | DICOM | [tcia_pseudo_phi_deid](tcia_pseudo_phi_deid.md) |
| ReproIn DICOM phantom | MR (multi-seq) | DICOM | [reproin_dicom_to_bids](reproin_dicom_to_bids.md) |
| OpenNeuro ds000102 Flanker | MR (T1w + BOLD) | BIDS/NIfTI | [openneuro_flanker_bids](openneuro_flanker_bids.md) |
| NiiVue demo images | various | NIfTI | [niivue_demo_images](niivue_demo_images.md) |
| Group-level FitLins Flanker | MR (T1w + BOLD) | BIDS | [fitlins_flanker_demo](fitlins_flanker_demo.md) |
| MSD Task04 Hippocampus | MR (T1w) | NIfTI + labels | [msd_hippocampus_nnunet](msd_hippocampus_nnunet.md) |
| TCIA QIN-PROSTATE smallest series | MR | DICOM | [tcia_collection_smallest](tcia_collection_smallest.md) |
| TCIA Mouse-Astrocytoma | MR (mouse) | DICOM | [tcia_mouse_astrocytoma_mri](tcia_mouse_astrocytoma_mri.md) |

## Cross-cutting themes

Some tutorial arcs span multiple datasets:

- **DICOM → NIfTI → BIDS** — `tcia_dicom_intro` → `dcm2niix` →
  `reproin_dicom_to_bids` → BIDS-Apps
- **Anonymization** — `tcia_pseudo_phi_deid` standalone
- **Single-subject vs group** — `openneuro_flanker_bids` (1 subject,
  hands-on) → `fitlins_flanker_demo` (group, automated)
- **Human → preclinical bridge** — `tcia_dicom_intro` →
  `tcia_mouse_astrocytoma_mri`, then jump to
  [container 04 RABIES](../04-rabies-rodent-fmri.md)
- **Segmentation** — `msd_hippocampus_nnunet` for nnU-Net,
  [container 02 MONAI bundle](../02-monai-bundle-segmentation.md) and
  [container 03 TS vs MONAI](../03-totalsegmentator-vs-monai.md) for
  full-body
- **Viewers** — `niivue_demo_images` is the easiest "open lots of
  volumes fast" project for viewer demos

## Using these in a course

1. Pre-load the relevant datasets via the plugin before the session
   (some pull from NBIA; allow time for the API call).
2. Walk through the *why* section first — most learners care about
   "what kind of data is this and why is it interesting?" before they
   want commands.
3. Run one container or pipeline from the linked **What to do with it**
   section.
4. Inspect outputs in the XNAT UI / a viewer.

The container walkthroughs at the parent
[tutorials/](../) directory complement these — the dataset docs
emphasize *data*, the container docs emphasize *workflow*.

## Status

Walkthroughs are first drafts — please refine for tone, depth, and
local site policies before instructor-led use.
