# Tutorial 06 — Preclinical Metadata as a Dynamic Data Type

Use a tiny non-imaging table from a rodent fMRI BIDS dataset to define a new
XNAT dynamic data type. This exercise is deliberately metadata-first: it shows
how XNAT can track cohort and acquisition descriptors that are not image files.

## Exercise Goal

Create a subject-assessor data type called `Rodent Acquisition Metadata`, add a
custom form based on the dataset dictionary, then enter one or more rows from
`participants.tsv`.

This pairs naturally with [Tutorial 04 — RABIES rodent fMRI preprocessing](04-rabies-rodent-fmri.md).

## Source Dataset

OpenNeuro `ds002551`, `Mouse_rest_awake`.

Mirrored non-imaging files:

- `datasets/openneuro/ds002551-metadata/participants.tsv`
- `datasets/openneuro/ds002551-metadata/participants.json`
- `datasets/openneuro/ds002551-metadata/dataset_description.json`

Original source:

- OpenNeuro dataset: `https://openneuro.org/datasets/ds002551`
- Raw S3 base: `https://s3.amazonaws.com/openneuro.org/ds002551`
- License: CC0, as reported by `dataset_description.json`

## Metadata Fields

The table has seven mouse participants and these columns:

| Column | Meaning | Example |
|---|---|---|
| `participant_id` | Mouse identifier | `SNT099` |
| `gender` | C57Bl/6J mouse sex | `m` |
| `field` | MRI field strength | `7` |
| `coil` | RF coil system | `Cryo` |
| `breathing` | Breathing condition | `f` |
| `sedation` | Sedation protocol | `Awake` |

The companion `participants.json` defines levels such as `m = Male`,
`Cryo = Bruker Cryoprobe`, `f = Free-breathing`, and `Awake = Awake (no sedation)`.

## Create The Data Type

1. Go to **Administer** -> **Data Types**.
2. Click **Create Data Type**.
3. Choose **Subject Assessor**.
4. Name it `Rodent Acquisition Metadata`.
5. Let XNAT auto-generate the namespace prefix and complex type.
6. Save the data type.

This should be a subject assessor because the fields describe the animal and
its acquisition setup, not a specific imaging resource or container output.

## Add The Custom Form

Create a custom form for `Rodent Acquisition Metadata`.

Suggested fields:

| Field label | Type | Required | Values |
|---|---|---|---|
| Participant ID | Text | yes | `SNT099`, `KKY921`, etc. |
| Sex | Single select | yes | `Male`, `Female` |
| Field strength | Number | yes | `7` |
| Coil | Single select | yes | `Bruker Cryoprobe` |
| Breathing | Single select | yes | `Free-breathing` |
| Sedation | Single select | yes | `Awake (no sedation)` |

## Enter A Record

Use the first row as a live workshop example:

| Field | Value |
|---|---|
| Participant ID | `SNT099` |
| Sex | `Male` |
| Field strength | `7` |
| Coil | `Bruker Cryoprobe` |
| Breathing | `Free-breathing` |
| Sedation | `Awake (no sedation)` |

Save the assessor under the matching subject if that subject exists. If the
subject does not exist yet, create a subject with label `SNT099` first.

## Verify

1. Open the subject report page.
2. Confirm the new `Rodent Acquisition Metadata` assessor is listed.
3. Open the assessor and confirm the custom form fields are populated.
4. Search for records where:
   - Field strength = `7`
   - Sedation = `Awake (no sedation)`
   - Coil = `Bruker Cryoprobe`

Expected result: the non-imaging cohort metadata is searchable inside XNAT
without adding a plugin or restarting the server.

## Discussion Points

- BIDS sidecar files are a practical source for custom XNAT metadata.
- Dynamic data types are useful for small, study-specific schemas that would
  otherwise live in spreadsheets.
- This exercise can later be automated by the tutorial plugin: load
  `participants.tsv`, create subjects, and instantiate one metadata assessor
  per row.
- For production studies, define controlled vocabularies up front and keep the
  BIDS data dictionary with the imported metadata.

