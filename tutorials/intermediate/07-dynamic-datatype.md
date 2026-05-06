# Dynamic data type

**Level:** Intermediate · **Time:** 25 min · **Recommended dataset:** `openneuro_ds002551_metadata`
(also works with any small project that has imaging sessions, for the
optional QC review variant)

## Goal

Add a new XNAT data type from the admin UI — without writing a plugin or
restarting the server — and use it to capture searchable metadata.

This lesson uses tabular preclinical metadata (`participants.tsv`) as the
worked example. The same pattern works for QC review forms, enrollment
forms, or any small study-specific schema. Dynamic data types are a fit
for **subject assessors**, **image assessors**, and **project assets**;
new image-session modalities still need plugin / schema work.

## Preconditions

- XNAT 1.10 or newer.
- Site administrator account on a training site.
- Custom Forms enabled.
- A project with the dataset loaded (or any sample project with at least
  one imaging session for the QC review variant).

## Walkthrough — Rodent Acquisition Metadata

1. Load `openneuro_ds002551_metadata`. Open the project resource that
   contains `participants.tsv` and `participants.json`.
2. Identify fields worth searching later:
   `participant_id`, `gender`, `field`, `coil`, `breathing`, `sedation`.
3. Open **Administer → Data Types**. Click **Create Data Type**.
4. Choose **Subject Assessor** (the metadata describes the animal, not a
   specific scan or run).
5. Name the type `Rodent Acquisition Metadata`. Let XNAT auto-generate the
   namespace prefix and complex type. Avoid reserved prefixes (`cat`,
   `prov`, `icr`, `scr`, `wrk`, `xnat`, `xnat_a`).
6. Save the data type.
7. Open **Tools → Custom Forms → Manage Data Forms**. Add a new form
   bound to `Rodent Acquisition Metadata`. Suggested fields:

   | Label | Type | Required | Values |
   |---|---|---|---|
   | Participant ID | Text | yes | `SNT099`, etc. |
   | Sex | Single select | yes | `Male`, `Female` |
   | Field strength | Number | yes | `7` |
   | Coil | Single select | yes | `Bruker Cryoprobe` |
   | Breathing | Single select | yes | `Free-breathing` |
   | Sedation | Single select | yes | `Awake (no sedation)` |

8. Save the form.
9. Create a subject `SNT099` if it does not exist, then add the new
   assessor and fill in the first row from `participants.tsv`.

## Expected result

The subject report shows a `Rodent Acquisition Metadata` assessor whose
fields are searchable through standard search and project tables.

## Verify

You can find the record by filtering on `Coil = Bruker Cryoprobe` or
`Field strength = 7`.

## If it does not work

- The new assessor is not searchable: confirm the custom form is bound to
  the same datatype (not a similarly-named older type) and that columns
  are exposed by your project's table view.
- The assessor option is missing from the subject Actions box: open
  **Administer → Data Types** and confirm the data type was saved with the
  add-action enabled.

## Variant — QC review form

Repeat the walkthrough with **Image Assessor** (not Subject Assessor),
type `Tutorial QC Review`, and four fields: `Review status` (select),
`Artifact severity` (select), `Needs follow-up` (boolean),
`Reviewer notes` (long text). Use any small imaging project — the
`tcia_dicom_intro` project works.

## Talking points

- Dynamic data types close the gap between ad hoc spreadsheets and
  full plugin development.
- Pick the parent type carefully: subject assessor for participant-level
  data, image assessor for session-level review or QC, project asset for
  project-level summaries.
- Standardise field names before a study starts. Renaming a live type is
  harder than adding one.

## Reference

- [Adding a Dynamic Data Type](https://wiki.xnat.org/documentation/adding-a-dynamic-data-type)
- [Creating a Custom Form](https://wiki.xnat.org/documentation/creating-a-custom-form)
