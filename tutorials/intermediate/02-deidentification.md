# De-identification

**Level:** Intermediate · **Time:** 20 min · **Recommended dataset:** `tcia_pseudo_phi_deid`

## Goal

Compare DICOM headers before and after an anonymisation policy, and
recognise the gotchas that policy-only thinking misses.

The Pseudo-PHI dataset contains synthetic PHI in the *places* PHI shows up
in real clinical DICOMs, so any anonymisation pipeline can be checked
against a known-failure-mode source.

## Preconditions

- A training XNAT (do not run this on production).
- Site or project anonymisation policy enabled.
- Admin or project-owner rights.

## Walkthrough

1. Load `tcia_pseudo_phi_deid` into `XNAT_TUTORIAL_DEID`.
2. Open the CT scan. Open one DICOM file's headers / tags view.
3. List fields that look like PHI:
   - Direct identifiers: `PatientName`, `PatientID`, `PatientBirthDate`.
   - Quasi-identifiers: `StudyDate`, `AccessionNumber`,
     `InstitutionName`.
   - Free text: `SeriesDescription`, `ImageComments`, private vendor tags.
4. Decide for each whether it should be **removed**, **replaced**,
   **shifted** (dates), or **kept** (e.g. modality, body part).
5. As an admin, open **Administer → Site Administration → DICOM** and
   review the active anonymisation script. Compare the rules to the field
   list.
6. Re-import the dataset (or archive a fresh prearchive row) with the
   policy enabled.
7. Open the new DICOM headers. Compare.

## Expected result

You can describe **three** fields the policy modified, plus at least one
gotcha that policy alone does not solve:

- Burned-in PHI in pixel data (handled by OCR-aware tools, not header
  scripts).
- Vendor private tags that policies often miss by default.
- Inferred PHI: combinations like institution + acquisition date can
  re-identify even after direct identifiers are gone.

## Verify

You can name three DICOM fields the policy affected on this dataset.

## If it does not work

- If the policy did not affect any field, confirm it is enabled at the
  scope you re-imported into (site or project).
- If headers look unchanged but the loader ran, you may be looking at the
  pre-policy import. Use the **after** project, not the **before** one.

## Talking points

- Anonymisation is a process, not a flag. HIPAA, GDPR, and NHS standards
  require different scrub levels.
- For batch CTP-style anonymisation with auditable rules, look at the
  `ctp_anon` container if your site has it.

## Reference

- [How To Use XNAT: DICOM Anonymization](https://wiki.xnat.org/documentation/how-to-use-xnat)
- [TCIA Pseudo-PHI-DICOM-Data](https://www.cancerimagingarchive.net/collection/pseudo-phi-dicom-data/)
