# FitLins Flanker demo — group-level GLM

## Why this dataset

Same scientific basis as [`openneuro_flanker_bids`](openneuro_flanker_bids.md)
— OpenNeuro ds000102 — but loaded **at group scale** (multiple subjects
on import) so you can demo first- and second-level analysis end-to-end
through FitLins inside XNAT.

FitLins is a BIDS-App that consumes a `model.json` (BIDS Stats Models
spec), runs subject-level GLMs, and combines them at the group level.
It's the closest thing to a *fully reproducible* fMRI analysis: the
model file describes contrasts and combinations declaratively, the BIDS
input is fixed, the container is pinned — re-running on a new subject
is a single command.

| | |
|---|---|
| Workflow | First-level + group-level GLM |
| License | PDDL |
| Source | [OpenNeuro ds000102](https://openneuro.org/datasets/ds000102) |
| Plugin id | `fitlins_flanker_demo` |
| Loader | `grouplevel_fitlins_flanker` (Group-Level plugin import) |
| Default project | `XNAT_TUTORIAL_FITLINS` |

## Download via the tutorial plugin

**UI** — Admin → **Tutorial Datasets** (`${XNAT_HOST}/xnat-tutorial/datasets.html`).
Find `fitlins_flanker_demo` in the list, click **Prepare**, set the project id, submit.

**REST** (admin auth required):

```bash
# stage source files only — leaves them in the plugin staging area
curl -u ${XNAT_USER}:${XNAT_PASS} -X POST \
  ${XNAT_HOST}/xapi/tutorials/datasets/fitlins_flanker_demo/download

# stage + create the project + import (typical)
curl -u ${XNAT_USER}:${XNAT_PASS} -X POST \
  "${XNAT_HOST}/xapi/tutorials/datasets/fitlins_flanker_demo/prepare?projectId=XNAT_TUTORIAL_FITLINS"
```

The Group-Level plugin handles import: creates subjects, MR sessions, T1w
and BOLD scans, NIfTI image resources, and BIDS metadata for the entire
group.

## What you get in XNAT

- Project `XNAT_TUTORIAL_FITLINS`
- N subjects, 1 session each, T1w + Flanker BOLD per session
- BIDS metadata wired through — every session is independently
  preprocessable and the project resolves as a BIDS dataset for
  group-level work

## Walkthrough

1. Show the project's subject list — explain why "1 dataset, many
   subjects" needs a different ingest pattern (Group-Level plugin) than
   single-session DICOM upload.
2. Run **fMRIPrep** on all subjects (or a pre-run cache).
3. Run **FitLins** with a Flanker `model.json`:
   - First level: contrast `incongruent − congruent` per subject.
   - Group level: one-sample t-test across the contrast.
4. Inspect the group activation map: anterior cingulate + dlPFC, the
   classic Flanker conflict signature.

## Talking points

- BIDS Stats Models is the FAIR analogue to FreeSurfer recon-all stats
  files: a contract for how analyses are specified.
- FitLins demonstrates that XNAT can host *and* run group analyses —
  not just a DICOM bucket.
- For sites with their own task data, the same FitLins wrapper takes any
  BIDS dataset with a model.json — no code changes.
