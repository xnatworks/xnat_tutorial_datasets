# DICOM import and archive

**Level:** Intro · **Time:** 15 min · **Recommended dataset:** `tcia_dicom_intro`
(also works with `tcia_collection_smallest`, `tcia_pseudo_phi_deid`)

## Goal

See how XNAT routes incoming DICOM into project / subject / session / scan
records, and learn the difference between **prearchive** and **archive**.

## Walkthrough

1. Load `tcia_dicom_intro` into `XNAT_TUTORIAL_DICOM`.
2. Open **Upload → Go to Prearchive**.
3. Look for a row matching `XNAT_TUTORIAL_DICOM`.
4. If the row is present and ready, select it and choose **Archive** (or the
   commit action your site uses). If no row is visible, auto-archive has
   already moved the session into the project — that is fine.
5. Open `XNAT_TUTORIAL_DICOM` and find the imported subject and session.
6. Open the session, the scan table, and the scan-level `DICOM` resource.

## Expected result

You can describe the difference between data sitting in prearchive (staged,
not yet part of the project) and data committed to the archive (visible to
the project, scan-level resources available).

## Verify

The tutorial project contains an archived imaging session with a `DICOM`
resource on its scan.

## If it does not work

- A stuck prearchive row usually means a routing or auto-archive setting
  problem; archive it manually and continue.
- If the prearchive is empty *and* the project is empty, rerun the loader.

## Reference

- [Image Session Upload Methods in XNAT](https://wiki.xnat.org/documentation/image-session-upload-methods-in-xnat)
- [Using the Prearchive](https://wiki.xnat.org/documentation/using-the-prearchive)
