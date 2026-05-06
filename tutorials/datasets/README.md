# Dataset reference cards

One card per dataset shipped by the
[XNAT Tutorial Plugin](https://github.com/xnatworks/xnat_tutorial_plugin).
Each card lists what the dataset contains, how to load it, and which
lessons recommend it. **Lessons live in [`../`](../) — these cards are
references, not tutorials.**

The plugin manifest (`config/tutorial-datasets.example.yml`) defines
upstream sources, fallback mirrors in this repo, ingest mechanics, and
project defaults. These cards are the human-readable companion. For
upstream sources, licenses, and mirrored-file inventories see
[`../../DATASETS.md`](../../DATASETS.md).

## Cards

| Dataset id | What it is | Format |
|---|---|---|
| [`tcia_dicom_intro`](tcia_dicom_intro.md) | Smallest "real DICOM" — TCIA QIN-PROSTATE MR series. | DICOM |
| [`tcia_collection_smallest`](tcia_collection_smallest.md) | Same series, loaded by collection query. | DICOM |
| [`tcia_pseudo_phi_deid`](tcia_pseudo_phi_deid.md) | TCIA Pseudo-PHI-DICOM — synthetic PHI for de-identification teaching. | DICOM |
| [`tcia_mouse_astrocytoma_mri`](tcia_mouse_astrocytoma_mri.md) | Preclinical mouse MRI from TCIA Mouse-Astrocytoma. | DICOM |
| [`reproin_dicom_to_bids`](reproin_dicom_to_bids.md) | ReproIn-encoded DICOM phantom for HeuDiConv-style conversion. | DICOM |
| [`bidscoin_dicom_to_bids`](bidscoin_dicom_to_bids.md) | BIDScoin tutorial DICOM (11 series) for the complete BIDS workflow. | DICOM |
| [`openneuro_flanker_bids`](openneuro_flanker_bids.md) | One-subject Flanker BIDS slice from OpenNeuro ds000102. | BIDS / NIfTI |
| [`openneuro_ds002551_metadata`](openneuro_ds002551_metadata.md) | Rodent participant metadata (TSV + JSON dictionary). | tabular |
| [`niivue_demo_images`](niivue_demo_images.md) | ~70 NIfTI volumes — atlases, templates, multi-modal samples. | NIfTI |
| [`msd_hippocampus_nnunet`](msd_hippocampus_nnunet.md) | MSD Task04 Hippocampus, the canonical nnU-Net starter. | NIfTI + labels |
| [`fitlins_flanker_demo`](fitlins_flanker_demo.md) | Multi-subject Flanker BIDS for group-level GLM demos. | BIDS (group) |

## Cross-cutting themes

Some lesson arcs span several datasets:

- **DICOM → NIfTI → BIDS** — `tcia_dicom_intro` →
  [intermediate/01](../intermediate/01-dcm2niix.md) →
  `reproin_dicom_to_bids` or `bidscoin_dicom_to_bids` →
  [advanced/01](../advanced/01-complete-bids.md).
- **Anonymisation** — `tcia_pseudo_phi_deid` standalone in
  [intermediate/02](../intermediate/02-deidentification.md).
- **Single-subject vs group** —
  `openneuro_flanker_bids` (1 subject) →
  `fitlins_flanker_demo` (group) →
  [advanced/06](../advanced/06-fitlins-group.md).
- **Human → preclinical bridge** —
  `tcia_dicom_intro` → `tcia_mouse_astrocytoma_mri` →
  [advanced/05 RABIES](../advanced/05-rabies-rodent-fmri.md).
- **Dynamic metadata** —
  `openneuro_ds002551_metadata` →
  [intermediate/07](../intermediate/07-dynamic-datatype.md) and
  [intermediate/08](../intermediate/08-custom-forms.md).
- **Segmentation** — `msd_hippocampus_nnunet` for nnU-Net
  ([advanced/04](../advanced/04-nnunet-dataset.md)) plus
  [advanced/02](../advanced/02-monai-segmentation.md) and
  [advanced/03](../advanced/03-segmentation-comparison.md) for
  full-body.
- **NIfTI viewing** — `niivue_demo_images` is the easiest "open lots
  of volumes fast" project for **Open with Workbench** demos
  ([intro/09](../intro/09-niivue-overlays.md)). OHIF is DICOM-only and
  does not open project-level NIfTI files.
