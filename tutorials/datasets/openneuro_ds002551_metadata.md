# `openneuro_ds002551_metadata` — rodent participant metadata

Non-imaging slice from OpenNeuro ds002551 (`Mouse_rest_awake`). A tiny
`participants.tsv` plus its BIDS data dictionary — ideal for teaching
why XNAT sometimes needs structured project-specific metadata in
addition to image data.

| | |
|---|---|
| Modality | tabular metadata |
| Format | TSV + JSON data dictionary |
| License | CC0 |
| Source | [OpenNeuro ds002551](https://openneuro.org/datasets/ds002551) |
| Plugin id | `openneuro_ds002551_metadata` |
| Default project | `XNAT_TUTORIAL_RODENT` |
| Files | `dataset_description.json`, `participants.tsv`, `participants.json` |

## Load it

UI — **Tools → Tutorial Datasets**, find `openneuro_ds002551_metadata`,
**Prepare**.

REST — see
[reference/rest-cheatsheet § Tutorial dataset loader](../reference/rest-cheatsheet.md#tutorial-dataset-loader).

## What you get in XNAT

- Project `XNAT_TUTORIAL_RODENT`.
- Project-level `TUTORIAL_METADATA` resource containing the three files.
- 7 mouse participant rows in `participants.tsv` covering
  `participant_id`, `gender`, `field`, `coil`, `breathing`, `sedation`.
- **No** imaging sessions — this is metadata-only by design.

## Lessons that use this dataset

- Intermediate: [07 dynamic data type](../intermediate/07-dynamic-datatype.md),
  [08 custom forms](../intermediate/08-custom-forms.md).
- Pair with `tcia_mouse_astrocytoma_mri` for a combined cohort-metadata
  + imaging story, or with
  [advanced/03 RABIES](../advanced/03-rabies-rodent-fmri.md) for the
  full preclinical fMRI pipeline.
