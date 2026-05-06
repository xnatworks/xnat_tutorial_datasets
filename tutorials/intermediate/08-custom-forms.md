# Custom forms

**Level:** Intermediate · **Time:** 20 min · **Recommended dataset:** `openneuro_ds002551_metadata`
(also works with `tcia_mouse_astrocytoma_mri`)

## Goal

Build a custom form, attach it to an existing dynamic data type, and
confirm the form's values are searchable / filterable / exportable.

This lesson is the natural follow-on to
[intermediate/07-dynamic-datatype](07-dynamic-datatype.md), which creates
the data type. Custom forms are how users actually enter the structured
metadata.

## Preconditions

- A dynamic data type already exists (from the previous lesson, or
  pre-created by an instructor).
- Custom Forms enabled at the site.
- Admin or project-owner rights.

## Walkthrough

1. Open **Tools → Custom Forms → Manage Data Forms**. Click **Add New**.
2. Title the form, e.g. `Rodent Acquisition Metadata`.
3. Bind it to the dynamic data type from the previous lesson.
4. Choose scope: site-wide for re-use across projects, or project-scoped
   for a single training course.
5. In the form builder, drag two or three components:
   - Text or number for `participant_id` / `field`.
   - Single-select for `coil`, `breathing`, `sedation`. Use the levels
     from `participants.json` (e.g. `Awake (no sedation)`,
     `Bruker Cryoprobe`).
6. Save the form.
7. Open a target subject. Create the assessor and confirm the form
   appears with your fields.
8. Fill the form for one or two participants from `participants.tsv`.

### Search the values

1. Open the project's data table or a stored search.
2. Add columns from the custom form (sites that expose them) or use
   standard search to find a known value.
3. Filter for `Coil = Bruker Cryoprobe`. Open a matching record.
4. Confirm the form values are visible on the report page.
5. Export the table if export is available on your site.

## Expected result

A reusable form that produces consistent, searchable metadata records.
Custom-form values are visible on the assessor report and discoverable
through search.

## Verify

You can locate one record using a value entered through the form, and
re-open the form to edit that record.

## If it does not work

- Form does not appear when creating an assessor: confirm the form is
  bound to the same data type, and that scope (site vs project) matches
  the project you are working in.
- Values are not searchable: open standard search and check whether your
  site exposes form fields as searchable columns. If not, table-based
  filtering on a stored search is the fallback.

## Talking points

- Forms bind UX to schema: the data type is what XNAT stores; the form is
  what the user sees. Changing the form does not migrate existing data.
- For published studies, freeze field labels and selects before
  collecting data. Renaming a form field after data exists is messy.

## Reference

- [Creating a Custom Form](https://wiki.xnat.org/documentation/creating-a-custom-form)
- [Using the Standard Search](https://wiki.xnat.org/documentation/using-the-standard-search)
