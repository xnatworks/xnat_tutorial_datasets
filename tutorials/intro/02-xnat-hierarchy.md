# Walk the XNAT hierarchy

**Level:** Intro · **Time:** 10 min · **Recommended dataset:** `tcia_dicom_intro`
(also works with `tcia_mouse_astrocytoma_mri`)

## Goal

Be able to point at every level XNAT uses to organise data: project,
subject, session, scan, resource.

## Walkthrough

1. Load `tcia_dicom_intro` into project `XNAT_TUTORIAL_DICOM` using
   [intro/01-load-sample-data](01-load-sample-data.md).
2. Open the project page. Note the title, ID, and accessibility line.
3. Open the subject list and click the first subject.
4. Open the imaging session listed under that subject.
5. Find the scan table on the session report page.
6. Click the first scan.
7. On the scan page, find the resources panel and open the `DICOM` resource.
8. Open one DICOM file's preview or metadata view.

## Expected result

You can name what is stored at each level — Project / Subject / Session /
Scan / Resource — and find a real file at the bottom.

## Verify

The project has at least one subject, one session, one scan, and one
scan-level `DICOM` resource that contains files.

## If it does not work

- If the project has no subjects, the loader may have failed: rerun
  [intro/01-load-sample-data](01-load-sample-data.md).
- If the session is missing but the loader said success, look for it in the
  prearchive — see [intro/03-dicom-import-archive](03-dicom-import-archive.md).

## Reference

[Understanding the XNAT Data Model](https://wiki.xnat.org/documentation/understanding-the-xnat-data-model)
