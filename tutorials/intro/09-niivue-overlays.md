# Atlases and overlays in NiiVue

**Level:** Intro · **Time:** 15 min · **Recommended dataset:** `niivue_demo_images`

## Goal

Distinguish base images from atlas / label / overlay images, and load both
into a viewer.

## Walkthrough

1. Load `niivue_demo_images` into `XNAT_TUTORIAL_NIIVUE`.
2. Open the project resources. The volumes import as a project-level
   `NIFTI` resource — there are no DICOM scans here.
3. Pick three contrasting volumes, for example:
   - `chris_t1.nii.gz` — anatomical T1.
   - `AllenAtlas.nii.gz` — atlas / label volume.
   - `fmri_pitch.nii.gz` — 4D activation map.
4. Launch the viewer (NiiVue if installed, otherwise OHIF).
5. Load the base image first, then the atlas as an overlay.
6. Adjust opacity, label colours, or sync settings if available.

## Expected result

You can name which file is the anatomy, which file is atlas data, and
which file is a derived activation map.

## Verify

You can identify one base image and one overlay or label volume by file
name.

## If it does not work

- If volumes are missing from the resource, re-run the loader.
- If your viewer does not support overlays, this lesson works as a
  file-listing tour even without a multi-volume viewer.

## Reference

[Using the XNAT OHIF Viewer](https://wiki.xnat.org/xnat-ohif-viewer/using-the-xnat-ohif-viewer)
