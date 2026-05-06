# Atlases and overlays — Open with Workbench

**Level:** Intro · **Time:** 15 min · **Recommended dataset:** `niivue_demo_images`

## Goal

Distinguish base images from atlas / label / overlay images, and load
both into a NIfTI-aware viewer using **Open with Workbench**.

## Preconditions

- The [Workbench plugin](https://github.com/xnatworks/xnat_workbench) is
  installed. NIfTI files in XNAT resources expose an **Open with
  Workbench** action when this plugin is present; without it, the
  resource is still browsable but you will not get an integrated viewer
  for raw NIfTI files. (OHIF is DICOM-first and is not the right tool
  for loading project-level NIfTI volumes directly.)

## Walkthrough

1. Load `niivue_demo_images` into `XNAT_TUTORIAL_NIIVUE`.
2. Open the project resources. The volumes import as a project-level
   `NIFTI` resource — there are no DICOM scans here.
3. Browse the resource file list. Pick three contrasting volumes, for
   example:
   - `chris_t1.nii.gz` — anatomical T1.
   - `AllenAtlas.nii.gz` — atlas / label volume.
   - `fmri_pitch.nii.gz` — 4D activation map.
4. On the base image (`chris_t1.nii.gz`), use **Open with Workbench**.
   The Workbench viewer opens with the volume loaded.
5. From the Workbench overlay panel (or by returning to the resource
   list and using **Open with Workbench → add as overlay** on the next
   file), load `AllenAtlas.nii.gz` as an overlay on top of the T1.
6. Repeat for `fmri_pitch.nii.gz` to see how a 4D activation map renders
   as a separate volume.
7. Adjust opacity, label colours, or sync settings.

## Expected result

You can name which file is the anatomy, which file is atlas data, and
which file is a derived activation map — and see them composited in
Workbench.

## Verify

You can identify one base image and one overlay or label volume by file
name, and toggle the overlay on / off in Workbench.

## If it does not work

- **No "Open with Workbench" action** on a NIfTI file: the Workbench
  plugin is not installed. Ask your admin to install it, or fall back to
  downloading the NIfTI and opening it in a local viewer (3D Slicer,
  ITK-SNAP, NiiVue desktop).
- **Volumes missing from the resource**: re-run the loader.

## Reference

- [Workbench plugin](https://github.com/xnatworks/xnat_workbench) —
  XNAT-integrated NIfTI viewer used by **Open with Workbench**.
- [Using the XNAT OHIF Viewer](https://wiki.xnat.org/xnat-ohif-viewer/using-the-xnat-ohif-viewer)
  — DICOM-focused viewer; for context, not used in this lesson.
