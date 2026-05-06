# Load sample data into XNAT

**Level:** Intro · **Time:** 5 min · **Recommended dataset:** any from
[../datasets/](../datasets/)

## Goal

Get a tutorial dataset into XNAT before the rest of the lessons need it.

## Walkthrough

1. Log in to XNAT.
2. Open **Tools → XNAT Tutorial & Walkthrough**, or open any project's
   **Actions → Load Tutorial Data**.
3. Pick the dataset the lesson recommends. Most lessons use
   `tcia_dicom_intro` for the first run.
4. Use the suggested project ID unless your course gives a different one.
5. Click **Prepare** and wait for the loader to report success.
6. Open the project from the project list.

## Expected result

The loader summary lists the project, subject, session, scan, or resource
counts that were created.

## Verify

The project appears in **Browse → Projects** and contains at least one
subject, session, scan, or named resource.

## If it does not work

- Check the loader status panel for an error.
- If the loader says the session is in **prearchive**, open
  [intro/03-dicom-import-archive](03-dicom-import-archive.md) and archive it
  from there.
- If your XNAT does not have the tutorial plugin installed, ask an admin to
  preload the dataset using the
  [REST loader](../reference/rest-cheatsheet.md#tutorial-dataset-loader),
  then start the lesson at its first XNAT UI step.
