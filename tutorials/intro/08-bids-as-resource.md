# BIDS as an XNAT resource

**Level:** Intro · **Time:** 15 min · **Recommended dataset:** `openneuro_flanker_bids`

## Goal

Recognise a BIDS dataset stored as an XNAT resource and tell the difference
between a BIDS resource (a file tree following BIDS conventions) and a
DICOM scan table.

## Walkthrough

1. Load `openneuro_flanker_bids` into `XNAT_TUTORIAL_BIDS`.
2. Open the project, then the session.
3. Open the session-level `BIDS` resource.
4. Open `dataset_description.json` (dataset-level metadata).
5. Open `participants.tsv` (subject-level metadata).
6. Navigate into `sub-01/anat/` and find the T1w NIfTI.
7. Navigate into `sub-01/func/` and find the BOLD NIfTI plus the
   `*_events.tsv` file.

## Expected result

You can point at a dataset-level, subject-level, and run-level BIDS file in
the resource tree and explain why a BIDS app reads them in that order.

## Verify

You can locate `dataset_description.json`, `participants.tsv`, one
`*_T1w.nii.gz`, and one `*_events.tsv`.

## If it does not work

- If the resource only shows a few files, the BIDS layout did not import
  cleanly. Re-run the loader and pick this dataset again.
- If you are looking at a scan table rather than a resource tree, you are
  on the session report page. Open the session's resources panel instead.

## Reference

[Strategies for XNAT Image Data Storage](https://wiki.xnat.org/documentation/strategies-for-xnat-image-data-storage)
