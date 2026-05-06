# Preclinical MRI

**Level:** Intermediate · **Time:** 20 min · **Recommended dataset:** `tcia_mouse_astrocytoma_mri`
(pair with `openneuro_ds002551_metadata` for the metadata side)

## Goal

Confirm that XNAT's data model — project / subject / session / scan /
resource — works unchanged for preclinical imaging, and recognise which
metadata fields are missing by default.

## Walkthrough

1. Load `tcia_mouse_astrocytoma_mri` into a tutorial project.
2. Walk the hierarchy
   ([intro/02-xnat-hierarchy](../intro/02-xnat-hierarchy.md)) on the
   mouse session. Open the scan, then the `DICOM` resource.
3. Open one DICOM file and inspect the headers.
4. Note which fields the scanner records: modality, body part, field
   strength, manufacturer.
5. Note which fields preclinical work usually wants but **DICOM does not
   carry by default**: species, strain, anaesthesia, coil model, atlas
   space.
6. Open a viewer and compare the spatial scale to a human dataset.
   For DICOM (mouse session and `tcia_dicom_intro`), use OHIF. For
   NIfTI (`niivue_demo_images`), use **Open with Workbench** — OHIF
   does not open NIfTI files.
7. Decide which preclinical metadata fields you would capture as a
   dynamic data type
   ([intermediate/07-dynamic-datatype](07-dynamic-datatype.md)).

## Expected result

You can describe what XNAT stores for free vs what a preclinical study
needs to capture explicitly, and which approach (custom forms, dynamic
data types, project resources) fits each gap.

## Verify

You can list one imaging field stored automatically (e.g. field strength
or modality) and one preclinical field that is not (e.g. anaesthesia or
strain).

## If it does not work

- DICOM tags say "MR" but the body part field is empty: that is normal
  for many preclinical exports; species and body-part metadata are often
  kept outside the DICOM header.
- The viewer struggles with very small voxel sizes: many viewers default
  to human-scale thresholds. Use the viewer's spacing override or load
  the volume in a small-animal viewer.

## Talking points

- Preclinical scanners (Bruker, Agilent, Varian) can export DICOM, but
  the metadata richness varies. Many sites keep raw vendor formats as
  scan-level resources alongside the DICOM.
- A dynamic data type is the lightest-weight way to add cohort,
  acquisition, or cohort-management fields without writing a plugin.

## Reference

- [Strategies for XNAT Image Data Storage](https://wiki.xnat.org/documentation/strategies-for-xnat-image-data-storage)
- [TCIA Mouse-Astrocytoma](https://www.cancerimagingarchive.net/collection/mouse-astrocytoma/)
