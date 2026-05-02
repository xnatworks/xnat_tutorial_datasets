# Tutorial 02 — Dynamic Data Type and Custom Form

Create a lightweight XNAT data type during a workshop, then attach a custom
form so users can collect searchable review fields without writing a plugin or
restarting XNAT.

Reference: XNAT wiki, [Adding a Dynamic Data Type](https://wiki.xnat.org/documentation/adding-a-dynamic-data-type).

## Exercise Goal

Define a simple image assessor called `Tutorial QC Review` and make it usable
from an image session's actions box. Add a custom form with a few quality
review fields, create one review on a sample session, and verify that the new
fields are searchable.

This is intentionally not a new image-session modality. Dynamic data types are
best for simple subject assessors, image assessors, and project assets. New
image session modalities still require plugin/schema development.

## Dataset

Use any small project that already has an image session. Recommended workshop
dataset:

- Plugin dataset: `tcia_dicom_intro`
- Default project: `XNAT_TUTORIAL_DICOM`
- Fallback mirror:
  `datasets/xnat-tutorial/tcia_dicom_intro/archive.zip`

If the tutorial plugin is installed, load **TCIA QIN-PROSTATE-Repeatability MR
series** from the dataset downloader before starting this exercise.

## Preconditions

1. XNAT 1.10.0 or newer.
2. Site administrator account.
3. One project with at least one image session.
4. Custom Forms enabled on the site.

## Beginner Guidance

What this tutorial does: adds a small new data object type to XNAT and gives it
a form so users can record structured QC review values. This is metadata
configuration, not image processing.

What to look for before starting: you are logged in as a site administrator,
the training site allows Dynamic Data Types, and the sample project has at
least one image session where an image assessor can be created.

How to know it worked: the session Actions box offers **Add Tutorial QC
Review**, saving the form creates an assessor under the session, and the review
fields can be searched or filtered.

What to check first if it fails: confirm the dynamic datatype was created as an
**Image Assessor**, confirm the action-box option was enabled, and confirm the
custom form is attached to the same datatype you created.

## Create The Data Type

1. Go to **Administer** -> **Data Types**.
2. Click **Create Data Type**.
3. Choose **Image Assessor**.
4. Name it `Tutorial QC Review`.
5. Let XNAT auto-generate the element name, complex type, and namespace prefix.
6. Leave advanced naming settings alone unless you are teaching schema naming.
7. Enable the option to add an **Add Tutorial QC Review** action to image
   session actions boxes.
8. Save.

Avoid reserved prefixes such as `cat`, `prov`, `icr`, `scr`, `wrk`, `xnat`,
and `xnat_a`.

## Add A Custom Form

After saving the data type, create a custom form for it.

Suggested fields:

| Field label | Type | Required | Example values |
|---|---|---|---|
| Review status | Single select | yes | `Pass`, `Questionable`, `Fail` |
| Artifact severity | Integer or select | no | `0`, `1`, `2`, `3` |
| Needs follow-up | Boolean | no | `true`, `false` |
| Reviewer notes | Long text | no | Free-text QC note |

Keep the first exercise small. The point is to show that a site admin can
define searchable metadata quickly, not to design a production clinical form.

## Create A Review

1. Open a session in the sample project.
2. In the session actions box, click **Add Tutorial QC Review**.
3. Fill in the fields:
   - Review status: `Questionable`
   - Artifact severity: `2`
   - Needs follow-up: `true`
   - Reviewer notes: `Motion artifact visible; review before analysis`
4. Save the assessor.

## Verify

1. Return to the session report page.
2. Confirm the new assessor appears under the session.
3. Open the assessor and confirm the custom form values are present.
4. Use XNAT search or the data type listing to find `Tutorial QC Review`
   records with `Needs follow-up = true`.

Expected result: the assessor behaves like a normal XNAT data object, and the
custom fields are available without a server restart.

## Discussion Points

- Dynamic data types close the gap between ad hoc spreadsheet tracking and
  custom plugin development.
- Subject assessors fit subject-level clinical or behavioral measurements.
- Image assessors fit session-derived reads, QC forms, ratings, and lightweight
  analysis summaries.
- Project assets are useful for project-level reports or group outputs, but the
  default UI support is thinner than for subject/image assessors.
- For production use, standardize naming and field definitions before a study
  starts. Renaming a live data model is harder than adding one.

## Optional Extension

Repeat the exercise with a subject assessor:

- Name: `Tutorial Enrollment Form`
- Fields: cohort, consent date, visit window, notes
- Use case: teach subject-level metadata capture and search.

For a concrete non-imaging preclinical table, use
[Tutorial 03 — Preclinical Metadata as a Dynamic Data Type](03-preclinical-metadata-datatype.md).
