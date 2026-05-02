# XNAT Tutorials

Walkthrough tutorials that pair XNAT features, containers, and downloadable
sample datasets from this repository. Self-paced and instructor-led use
both supported.

## For New XNAT Users

Most tutorials use the same small set of XNAT objects:

| XNAT word | Plain meaning | What to inspect |
|---|---|---|
| Project | A study workspace that controls access and groups data. | Project page, project resources, project users. |
| Subject | A participant, patient, animal, or sample inside a project. | Subject report page and its sessions. |
| Session | One imaging visit or acquisition event. | Scan table, assessors, session resources. |
| Scan | One acquired image series inside a session. | DICOM/NIFTI resources and scan metadata. |
| Resource | A named file collection attached to a project, session, scan, or assessor. | File list, labels such as `DICOM`, `NIFTI`, `BIDS`, `DATA`. |
| Assessor | A derived result attached to a session, usually produced by processing or review. | Assessor page and output resources. |
| Container | A reproducible processing job launched through Container Service. | Command history, stdout/stderr logs, output resources. |

Each walkthrough should be readable without prior XNAT experience. When you
write or review a tutorial, make sure every major step answers:

- **What this step does**: what XNAT object or workflow is changed.
- **What to look for**: the page, table, resource, log line, or output that
  proves the learner is in the right place.
- **How to know it worked**: the concrete success condition before moving on.
- **What to check first if it fails**: the next useful page or log, not a vague
  "ask an admin" instruction.

## Numbered walkthroughs

The numbered path moves from easy to more advanced. It starts with short,
low-risk operations that teach XNAT objects and command outputs, then moves
into AI comparison, full BIDS orchestration, and longer preclinical pipelines.

| # | Tutorial | Category | Dataset / container | Walltime |
|---|---|---|---|---|
| 01 | [DICOM → NIfTI conversion](01-dcm2niix.md) | Container basics | `tcia_dicom_intro` + `xnatworks/dcm2niix:2.0` | < 1 min |
| 02 | [Dynamic Data Type and Custom Form](02-dynamic-data-type.md) | Admin metadata basics | `tcia_dicom_intro` sample project | 10–15 min |
| 03 | [Preclinical Metadata as a Dynamic Data Type](03-preclinical-metadata-datatype.md) | Metadata / preclinical | OpenNeuro `ds002551` `participants.tsv` | 10–15 min |
| 04 | [MONAI Bundle segmentation](04-monai-bundle-segmentation.md) | AI segmentation | CT abdomen/pelvis + `xnatworks/monai-bundle-nifti:0.3.0` | 5–15 s per scan |
| 05 | [TotalSegmentator vs MONAI comparison](05-totalsegmentator-vs-monai.md) | AI comparison | same CT as 04 + TotalSegmentator/MONAI | ~5 min total |
| 06 | [Complete BIDS workflow](06-complete-bids-walkthrough.md) | DICOM to BIDS to BIDS app | `bidscoin_dicom_to_bids` + BIDS demo pipeline | 20–45 min |
| 07 | [RABIES — rodent fMRI preprocess](07-rabies-rodent-fmri.md) | Preclinical BIDS | OpenNeuro `ds002551` + `ghcr.io/cobralab/rabies:0.6.0` | 10–60 min |

## Simple XNAT walkthroughs

The [simple walkthroughs](simple-walkthroughs.md) are user-facing
instructions for short XNAT lessons. Each one recommends a dataset from
this repository and gives concrete UI steps, expected results, and a
verification check.

## Dataset walkthroughs

One per dataset shipped by the
[xnat_tutorial_plugin](https://github.com/xnatworks/xnat_tutorial_plugin)
manifest. Focuses on **why the data matters** first, XNAT plumbing
second. See [`datasets/README.md`](datasets/README.md) for the index and
cross-cutting tutorial arcs.

For workshop planning, see the
[pick-and-mix tutorial catalog](pick-and-mix.md). It lists simple
tutorial topics and the datasets each tutorial should recommend, so the
same dataset can be reused across beginner, admin, BIDS, preclinical, or
dynamic datatype sessions. It also includes a practical use-case map that
starts from downloaded sample data and follows wiki-backed XNAT workflows.

Per-dataset upstream sources, licenses, and local-mirror fallbacks are
listed in [`sources.yml`](sources.yml).

Each tutorial is self-contained — pre-reqs, dataset download when needed,
step-by-step launch or admin workflow, expected outputs, and what to inspect.

## Running on any XNAT

Container tutorials assume an XNAT instance with Container Service installed
and Docker access for the running user. Admin/UI tutorials require the named
XNAT feature and a site administrator account. REST examples reference
`${XNAT_HOST}`, `${XNAT_USER}`, `${XNAT_PASS}`, `${PROJECT}`, `${SESSION}`,
`${SCAN}`, and `${WRAPPER_ID}` as placeholders — set them in your shell or
substitute inline.

## Common preconditions

1. XNAT 1.10 or newer for the full workshop path.
2. Tutorial plugin installed when you want one-click sample data loading.
3. Container Service plugin enabled for container exercises.
4. Docker host with GPU support where needed (CUDA 12+ recommended for
   MONAI/RABIES).
5. The relevant container command registered + enabled on the project. Each
   container tutorial shows the registration `curl` for its container.

## Dataset download

Datasets in this repo are course-sized subsets. Tutorials reference them
via:

```
https://raw.githubusercontent.com/xnatworks/xnat_tutorial_datasets/main/datasets/<path>
```

Larger source datasets are linked to their original homes (TCIA, OpenNeuro,
etc.) — also documented in the [main README](../README.md).

## Tutorial conventions

- Code blocks use `bash` — copy-paste runnable
- `${XNAT_HOST}` and `${XNAT_USER}:${XNAT_PASS}` are placeholders — set them
  in your shell first
- Screenshots link to public wiki.xnat.org pages when the walkthrough follows
  a wiki workflow
- Each tutorial should include beginner-facing **What this step does**,
  **What to look for**, **How to know it worked**, and **What to check if it
  does not** guidance near the workflow steps.
- Each tutorial ends with **Expected output** + **Verify** sections or an
  equivalent concrete success check so a learner can confirm a successful run.

## Maintenance

Tutorials live under `tutorials/`. Datasets they reference are mirrored
under `datasets/` (raw GitHub) and pulled live from upstream when
available. The plugin manifest at `tutorials/sources.yml` is the
authoritative dataset source list.
