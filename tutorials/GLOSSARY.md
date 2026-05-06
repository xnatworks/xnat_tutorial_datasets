# XNAT Glossary

The vocabulary every tutorial in this repo uses. Skim it before your first
lesson, then refer back when a tutorial mentions an object you don't recognise.

| XNAT word | Plain meaning | Where to find it |
|---|---|---|
| Project | A study workspace that controls access and groups data. | Top-bar **Browse → Projects**. |
| Subject | A participant, patient, animal, or sample inside a project. | Subject list on the project page. |
| Session | One imaging visit or acquisition event. | Subject report page → image sessions table. |
| Scan | One acquired image series inside a session. | Session report page → scan table. |
| Resource | A named file collection attached to a project, session, scan, or assessor. | "Manage Files" or the resources panel at any level. |
| Assessor | A derived result attached to a session or subject. | Session report → assessors section. |
| Container | A reproducible processing job launched through Container Service. | Project or session **Actions → Run Containers**, then command history. |
| Prearchive | A staging area where uploaded DICOM lands before being archived. | Top-bar **Upload → Go to Prearchive**. |
| BIDS resource | A `BIDS` resource label that holds a Brain Imaging Data Structure tree. | Session resources panel, `BIDS` label. |
| Custom form | A dynamic field set attached to an XNAT data type. | **Tools → Custom Forms → Manage Data Forms**. |
| Dynamic data type | A data type added through the admin UI without a plugin restart. | **Administer → Data Types**. |

## The four checkpoints

Every intro and intermediate lesson asks the same four questions. They are the
fastest way to confirm a step worked before moving on:

- **What this step changes** in XNAT.
- **Where to look** in the UI after the step.
- **The exact success condition** before moving on.
- **The first useful page or log to check** if the result is missing.

If a tutorial step does not give you all four, treat that as a gap to fix.
