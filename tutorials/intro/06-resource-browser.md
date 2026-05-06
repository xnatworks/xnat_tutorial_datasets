# Resource browser

**Level:** Intro · **Time:** 10 min · **Recommended dataset:** `openneuro_flanker_bids`
(works with any dataset)

## Goal

Understand that **resources** are named file collections that can attach to
a project, session, scan, or assessor — different levels store different
files for different reasons.

## Walkthrough

1. Load `openneuro_flanker_bids` into `XNAT_TUTORIAL_BIDS`.
2. Open the project page.
3. Find the project resources panel and open the `BIDS` resource.
4. Open `dataset_description.json`.
5. Go back to the project. Open a subject, then the session.
6. On the session page, open a scan resource if one exists, then a session
   resource.
7. Compare: which file lives at which level?

## Expected result

You can name three places resources can attach (project, session, scan)
and explain why a researcher might pick one over another.

## Verify

You can open one project-level file and one scan- or session-level file in
the same project.

## If it does not work

- If the project has no resources, the loader may still be running.
  Re-open the project after a minute.
- If you only see scan rows and no resource panel, you are looking at a
  session that hasn't been archived yet. See
  [intro/03-dicom-import-archive](03-dicom-import-archive.md).

## Reference

[Strategies for XNAT Image Data Storage](https://wiki.xnat.org/documentation/strategies-for-xnat-image-data-storage)
