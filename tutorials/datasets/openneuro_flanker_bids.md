# OpenNeuro ds000102 Flanker BIDS subset

## Why this dataset

ds000102 is one of the **earliest publicly shared task-fMRI datasets**
from OpenNeuro (formerly OpenfMRI), and is still the go-to teaching set
for the Eriksen Flanker conflict-monitoring paradigm. 26 subjects each
ran a single Flanker task run plus a T1w anatomical, all in BIDS layout
with proper events.tsv files. It's the dataset behind countless fMRIPrep,
FitLins, and FSL FEAT tutorials.

The Flanker task itself is a behavioral classic: subjects respond to a
central arrow flanked by congruent or incongruent arrows. The contrast
between congruent and incongruent trials is one of the most replicated
fMRI activations in the literature (anterior cingulate, dorsolateral
prefrontal cortex). Demos that produce visible activation in 10 minutes
on a single subject earn instant credibility.

| | |
|---|---|
| Modality | MR (T1w + BOLD) |
| Task | Eriksen Flanker |
| License | PDDL (older OpenNeuro convention; equivalent to public domain) |
| Source | [OpenNeuro ds000102](https://openneuro.org/datasets/ds000102) |
| Plugin id | `openneuro_flanker_bids` |
| Default project | `XNAT_TUTORIAL_BIDS` |
| Subjects (default) | 1 (sub-01) |

## Download via the tutorial plugin

**UI** — Admin → **Tutorial Datasets** (`${XNAT_HOST}/xnat-tutorial/datasets.html`).
Find `openneuro_flanker_bids` in the list, click **Prepare**, set the project id, submit.

**REST** (admin auth required):

```bash
# stage source files only — leaves them in the plugin staging area
curl -u ${XNAT_USER}:${XNAT_PASS} -X POST \
  ${XNAT_HOST}/xapi/tutorials/datasets/openneuro_flanker_bids/download

# stage + create the project + import (typical)
curl -u ${XNAT_USER}:${XNAT_PASS} -X POST \
  "${XNAT_HOST}/xapi/tutorials/datasets/openneuro_flanker_bids/prepare?projectId=XNAT_TUTORIAL_BIDS"
```

## What you get in XNAT

- Project `XNAT_TUTORIAL_BIDS`
- 1 subject (sub-01) with anatomical and 1 functional run
- BIDS resource at the session level (already in BIDS layout — no
  conversion needed)

## What to do with it

| Goal | Tool |
|---|---|
| Preprocess | `fMRIPrep` (already wrapped on most XNAT instances) |
| First-level GLM | `FitLins` — see [fitlins_flanker_demo.md](fitlins_flanker_demo.md) for the group-level version |
| QC | `MRIQC` |

## Workshop flow

1. Inspect the BIDS resource on the session. Show
   `dataset_description.json`, `participants.tsv`, the events.tsv with
   onset/duration/trial_type for each Flanker trial.
2. Launch fMRIPrep with `--participant_label sub-01 --output-spaces MNI152NLin2009cAsym`.
3. While it runs (~30 min), discuss BIDS-Apps as the pattern for
   reproducible neuroimaging.
4. View output: `sub-01_task-flanker_run-1_space-MNI152*_desc-preproc_bold.nii.gz`
   in OHIF/Workbench. Confluence point with the BIDS resource on the
   session means subsequent tools find inputs automatically.

## Talking points

- BIDS isn't just a folder layout; it's a **contract** that lets
  pipelines compose without configuration. fMRIPrep, FitLins, MRIQC,
  TractSeg, etc. all consume the same BIDS resource.
- ds000102 is small enough to land + preprocess + analyze in one
  workshop slot, but realistic enough that the activation maps are
  publishable in concept.
- The XNAT BIDS resource model lets you keep raw DICOMs on scans *and*
  the BIDS view on the session in the same archive — no fork.
