# Tutorial Container Commands

Curated XNAT Container Service command JSONs for tutorial workflows.

This folder is intentionally narrower than the general containers catalog:

- command JSONs must use publicly pullable Docker Hub images
- image references must use pinned tags, not `latest`
- command JSONs must be safe for tutorial installation on a workshop XNAT
- source commands should preserve upstream credit and be copied here only when
  the tutorial plugin can install them on demand

## Profiles

| Profile | Purpose | Commands |
|---|---|---|
| `bids_demo_pipeline` | ReproIn DICOM to BIDS preprocessing walkthrough | BIDS mapping, dcm2bids, MRIQC, fMRIPrep, QSIPrep |

The tutorial plugin uses `manifest.yml` to decide which commands are required
for a selected tutorial dataset.

