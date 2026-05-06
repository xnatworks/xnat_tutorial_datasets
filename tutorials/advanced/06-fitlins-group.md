# FitLins group analysis

**Level:** Advanced · **Time:** 30+ min ·
**Recommended dataset:** `fitlins_flanker_demo`

The natural follow-on to the single-subject Flanker BIDS lesson — same
task, but with enough subjects to teach group-level GLM concepts and the
XNAT data model for multi-subject analyses.

## Goal

Run (or review) FitLins on a multi-subject BIDS project, then explain
where group-level outputs belong in the XNAT data model.

## Preconditions

- Container Service installed.
- A FitLins wrapper enabled at the project. The exact image and wrapper
  versions live in your site's container registry — confirm before the
  session.
- A multi-subject BIDS dataset already imported. The
  `fitlins_flanker_demo` source exists in `sources.yml` but is not
  mirrored in this repo as a normal raw file (it is too large for direct
  GitHub fallback). Configure your loader to pull it from object storage
  or a release asset.

## Walkthrough

1. Open the project. Confirm a `BIDS` resource at session or project
   level with multiple `sub-*` directories.
2. Open one subject's anatomical and functional resources. Compare to
   another subject — group analysis depends on consistent layout across
   subjects.
3. Decide where group-level output should land in XNAT. Three options:
   - **Project-level resource**: simple, durable, easy to reference.
   - **Project-level assessor**: structured fields for status, errors,
     model summary; useful when you want to query group runs as records.
   - **Per-subject assessors plus a project-level summary resource**:
     useful when single-subject contrasts are also interesting.
4. Launch the FitLins wrapper. Watch command history.
5. Open the resulting output. Note that the **input** is a BIDS layout
   spanning subjects, while the **output** is one set of files
   describing the model and contrasts across the group.

## Expected output

A FitLins output resource (or assessor) attached at the project level,
containing model JSON, contrast NIfTIs, and reports for the group.

## Verify

You can open one model definition and one group-level contrast volume
from the FitLins output, and explain why the result is **not** attached
to a single subject's session.

## If it does not work

- **FitLins fails on BIDS validation**: run
  [intermediate/03-bids-validation](../intermediate/03-bids-validation.md)
  first.
- **Wrapper not visible**: FitLins is not bundled in this repo's manifest
  by default. Confirm with your site admin which BIDS-app wrappers are
  installed.

## Talking points

- Single-subject vs group: the *file* may look similar (a contrast
  NIfTI), but the *meaning* depends on the analysis level.
- Reproducibility for group-level analyses depends on every subject's
  preprocessing being comparable. Mixing fMRIPrep versions across
  subjects is a common quiet bug.
- The Flanker task is one of the most replicated fMRI activations in the
  literature — a believable group output is a quick credibility check on
  the pipeline.

## Reference

- [FitLins documentation](https://fitlins.readthedocs.io/)
- [OpenNeuro ds000102](https://openneuro.org/datasets/ds000102)
