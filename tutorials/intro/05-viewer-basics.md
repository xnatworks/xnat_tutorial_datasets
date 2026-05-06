# Open a scan in the viewer

**Level:** Intro · **Time:** 10 min · **Recommended dataset:** `tcia_dicom_intro`
(also works with `niivue_demo_images`, `tcia_mouse_astrocytoma_mri`)

## Goal

Launch the installed image viewer from XNAT and connect what you see in the
viewer to the scan it came from.

## Walkthrough

1. Load `tcia_dicom_intro` into `XNAT_TUTORIAL_DICOM`.
2. Open the imaging session.
3. In the session **Actions** box, choose the viewer action installed on
   your site. On OHIF-enabled sites this is **View Images**.
4. Wait for the viewer to load. Scroll through slices.
5. If the viewer shows multiple scans, pick one.
6. Return to XNAT in another tab and open the scan table.
7. Match the viewer's scan label to a row in the scan table.

## Expected result

The viewer opens with the scan, and you can name the scan ID it represents.

## Verify

Given the viewer image, you can find the same scan ID in XNAT's scan table.

## If it does not work

- No viewer action in the Actions box means no viewer is installed; ask
  your admin which viewer plugin is available.
- A blank viewer usually means the scan has no image files; open the
  `DICOM` resource and check.

## Reference

[Using the XNAT OHIF Viewer](https://wiki.xnat.org/xnat-ohif-viewer/using-the-xnat-ohif-viewer)
