# Search, filter, and export

**Level:** Intro · **Time:** 15 min · **Recommended dataset:** `openneuro_flanker_bids`
(also works with `tcia_dicom_intro`, `openneuro_ds002551_metadata`)

## Goal

Find a known project / subject / session quickly, narrow a list with
filters, and download a small file.

## Walkthrough

1. Load `openneuro_flanker_bids` into `XNAT_TUTORIAL_BIDS`.
2. Use the top-bar search box. Type the project ID, a subject label, or a
   session label.
3. If XNAT shows multiple matches, pick the matching row from the grouped
   results page.
4. Return to **Browse → Projects** (or use the **Subjects / MR / PET / CT**
   tabs) for a broader table-based search.
5. Open the project. Use the column filter on the subject or session table
   to narrow the list.
6. Open the session and the `BIDS` resource.
7. Download `participants.tsv` from the resource file list.
8. (Optional) Open the project's download action and review the format /
   scan type / method options without committing to a full download.

## Expected result

You can describe when the top-bar search wins (you know the exact label)
versus when a table search wins (you want to filter many rows).

## Verify

You have `participants.tsv` (or another small metadata file) downloaded
locally.

## If it does not work

- An empty search result usually means the indexer has not seen the new
  project yet. Re-load the page after a minute, or filter the project list
  directly.
- If the resource file does not download, open it inline first to confirm
  the file exists.

## Reference

- [Using the Standard Search](https://wiki.xnat.org/documentation/using-the-standard-search)
- [How To Download Image Data From XNAT Projects](https://wiki.xnat.org/documentation/how-to-download-image-data-from-xnat-projects)
