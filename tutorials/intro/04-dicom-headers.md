# Read DICOM headers

**Level:** Intro · **Time:** 15 min · **Recommended dataset:** `tcia_pseudo_phi_deid`
(also works with `tcia_dicom_intro`, `reproin_dicom_to_bids`)

## Goal

Open a DICOM file's tags and tell the difference between routing metadata
(used by XNAT to organise data) and patient metadata (privacy-sensitive).

## Walkthrough

1. Load `tcia_pseudo_phi_deid` into `XNAT_TUTORIAL_DEID`.
2. Open the project, the subject, and the CT session.
3. Open the scan, then the `DICOM` resource.
4. Click a DICOM file and open the headers / tag view.
5. Find these fields:
   - `PatientName`, `PatientID`, `PatientBirthDate` (patient metadata)
   - `StudyDate`, `AccessionNumber` (study metadata)
   - `Modality`, `SeriesDescription`, `SeriesNumber` (routing metadata)
6. Compare those values to the project, subject, session, and scan labels
   in the XNAT UI.

## Expected result

You can describe which fields XNAT used to file the data and which fields
look like sensitive patient information.

## Verify

You can name **`Modality`** and **`SeriesDescription`** in a header, and
point at one field that looks like PHI.

## If it does not work

- If the headers view is empty, you may be looking at a non-image file.
  Open a different file in the resource.
- If the DICOM resource is missing, archive the prearchive row first
  ([intro/03-dicom-import-archive](03-dicom-import-archive.md)).

## Reference

[How XNAT Scans DICOM to Map to Project/Subject/Session](https://wiki.xnat.org/documentation/how-xnat-scans-dicom-to-map-to-project-subject-ses)
