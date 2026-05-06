# `fitlins_flanker_demo` — group-level FitLins Flanker

Same scientific basis as
[`openneuro_flanker_bids`](openneuro_flanker_bids.md) — OpenNeuro
ds000102 — but loaded **at group scale** (multiple subjects on import)
so a tutorial can demo first- and second-level analysis end-to-end with
FitLins inside XNAT.

| | |
|---|---|
| Workflow | First-level + group-level GLM |
| Format | BIDS (multi-subject) |
| License | PDDL |
| Source | [OpenNeuro ds000102](https://openneuro.org/datasets/ds000102) |
| Plugin id | `fitlins_flanker_demo` |
| Loader | `grouplevel_fitlins_flanker` |
| Default project | `XNAT_TUTORIAL_FITLINS` |

## Load it

UI — **Tools → Group-Level Datasets**, find `fitlins_flanker_demo`,
**Prepare**.

REST — see
[reference/rest-cheatsheet § Tutorial dataset loader](../reference/rest-cheatsheet.md#tutorial-dataset-loader).

The Group-Level loader creates subjects, MR sessions, T1w + Flanker BOLD
scans, NIfTI image resources, and BIDS metadata for the entire group.

## Caveat — fallback availability

This dataset is large; it is **not** mirrored as a normal raw GitHub
file. For an offline-friendly fallback, publish the assembled archive
as a GitHub release asset or in object storage and configure the loader
source URL accordingly. See
[admin/03 plugin loader admin](../admin/03-plugin-loader-admin.md).

## What you get in XNAT

- Project `XNAT_TUTORIAL_FITLINS`.
- N subjects, 1 session each, T1w + Flanker BOLD per session.
- BIDS metadata wired through — every session is independently
  preprocessable, and the project resolves as a BIDS dataset for
  group-level work.

## Lessons that use this dataset

- Advanced: [04 FitLins group analysis](../advanced/04-fitlins-group.md).
