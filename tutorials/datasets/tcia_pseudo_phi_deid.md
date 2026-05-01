# TCIA Pseudo-PHI-DICOM-Data — anonymization teaching

## Why this dataset

This is the **canonical teaching dataset for DICOM de-identification**,
published by NIH/CIP. The "Pseudo-PHI" series intentionally contains
synthetic PHI in the *places* PHI typically appears in real clinical
DICOMs — patient name, date of birth, accession number, free-text fields,
private tags, burned-in pixel data. Researchers and IT admins use it to
verify that their anonymization pipelines actually scrub everything they
think they scrub.

It's a known-failure-mode teaching tool: every supposedly-anonymized
output you produce can be checked against a source that has known
PHI-shaped content in known places.

| | |
|---|---|
| Modality | CT |
| License | CC-BY-4.0 |
| Source | [TCIA Pseudo-PHI-DICOM-Data](https://www.cancerimagingarchive.net/collection/pseudo-phi-dicom-data/) |
| Plugin id | `tcia_pseudo_phi_deid` |
| Default project | `XNAT_TUTORIAL_DEID` |
| Series pulled | 2 DICOM files (ultra-tiny) |

## Download via the tutorial plugin

**UI** — Admin → **Tutorial Datasets** (`${XNAT_HOST}/xnat-tutorial/datasets.html`).
Find `tcia_pseudo_phi_deid` in the list, click **Prepare**, set the project id, submit.

**REST** (admin auth required):

```bash
# stage source files only — leaves them in the plugin staging area
curl -u ${XNAT_USER}:${XNAT_PASS} -X POST \
  ${XNAT_HOST}/xapi/tutorials/datasets/tcia_pseudo_phi_deid/download

# stage + create the project + import (typical)
curl -u ${XNAT_USER}:${XNAT_PASS} -X POST \
  "${XNAT_HOST}/xapi/tutorials/datasets/tcia_pseudo_phi_deid/prepare?projectId=XNAT_TUTORIAL_DEID"
```

## What you get in XNAT

- Project `XNAT_TUTORIAL_DEID`
- 1 subject, 1 CT scan, 2 DICOM files

## Walkthrough

1. **Look at the headers as imported** — XNAT scan page → DICOM Headers.
   Note PatientName, PatientBirthDate, PatientID, AccessionNumber, plus
   any private tags. These were intentionally populated.
2. **Apply XNAT anonymization** — site-level or project-level scripts.
   Triage anonymization, then promote to archive.
3. **Compare** — pull the same headers post-anonymization. Discuss what
   was kept vs scrubbed and *why* (date shifts, hash IDs, etc.).
4. **Talk about gotchas** — burned-in PHI in pixel data, private vendor
   tags, OCR for text overlays. Tools like `dicom-deid-ocr` (a container
   already on most XNAT demo instances) handle pixel-level PHI.

## Talking points

- Anonymization is a process, not a flag. Different jurisdictions
  (HIPAA, GDPR, NHS) require different scrub levels.
- The "Pseudo-PHI" naming convention is widely cited — searching for it
  in the literature lands on de-identification methodology papers.
- For research-data sharing under XNAT, pair with the `ctp_anon`
  container for batch CTP-based anonymization with auditable rules.
