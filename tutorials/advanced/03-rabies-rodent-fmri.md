# RABIES — rodent fMRI preprocessing

**Level:** Advanced · **Time:** 10–60 min (≈10–15 min with
`fast_commonspace=true`) · **Recommended dataset:** see
[`../sources.yml`](../sources.yml) entry `07-rabies-rodent-fmri`

The preclinical anchor: a published, BIDS-driven preprocessing pipeline
purpose-built for **rodent** fMRI/sMRI. Distinct from human-derived
pipelines (fMRIPrep, QSIPrep) — RABIES uses rodent atlases, anatomical
priors, and motion parameters tuned for the rodent brain.

| | |
|---|---|
| Image | `ghcr.io/cobralab/rabies:0.6.0` |
| Source | https://github.com/CoBrALab/RABIES |
| Reference | Desrosiers-Grégoire et al. *Nat Commun* 2024 ([doi:10.1038/s41467-024-50826-8](https://www.nature.com/articles/s41467-024-50826-8)) |
| GPU | optional (helps registration; CPU works) |

## Pipeline stages

| Stage | Subcommand | Wrapped on XNAT |
|---|---|---|
| Preprocess | `rabies preprocess` | yes — this lesson |
| Confound correction | `rabies confound_correction` | not yet |
| Analysis (FC, QC) | `rabies analysis` | not yet |

## Datasets

`sources.yml` lists:

- `07-rabies-rodent-fmri` — full BIDS rodent rs-fMRI (anatomy +
  functional) for an end-to-end demo.
- `07-rabies-rodent-fmri-anatomy-only` — anatomical-only mirror; useful
  as a sanity check for brain extraction and atlas registration.

## Expected BIDS layout

```
${BIDS_ROOT}/
└── sub-<id>/
    └── ses-<id>/                          # session optional
        ├── anat/sub-<id>_T2w.nii.gz       # or T1w
        └── func/sub-<id>_task-rest_bold.nii.gz
```

Each NIfTI needs a sidecar `*.json` with at minimum `RepetitionTime`.

XNAT BIDS pipelines (`bids-tree-builder`, `dcm2bids-session-v17`)
generate this from raw scans — run them first if a session does not yet
have a `BIDS` resource. See
[advanced/01-complete-bids](01-complete-bids.md).

## Preconditions

- Container Service installed.
- Image pulled (one-time, large):
  ```bash
  docker pull ghcr.io/cobralab/rabies:0.6.0
  ```
- Command registered (`command.json` lives in the
  [xnat_monailabel_container repo](https://github.com/xnatworks/xnat_monailabel_container/blob/main/preclinical/rabies/command.json)):
  ```bash
  SESS=$(curl -s -u ${XNAT_USER}:${XNAT_PASS} ${XNAT_HOST}/data/JSESSION)
  curl -b "JSESSIONID=$SESS" -X POST -H 'Content-Type: application/json' \
    -d @command.json \
    ${XNAT_HOST}/xapi/commands
  ```
- Wrapper enabled at site + project level.
- A session that already has a `BIDS` resource matching the layout above.

## Walkthrough

1. Open the session that has the `BIDS` resource.
2. Click **Run Containers** at the session level →
   **RABIES preprocess (BIDS resource)**.
3. Pick the `BIDS` resource. Set the TR. For a live demo, set
   `fast_commonspace=true`.
4. Click **Run**. Open command history.
5. When `Complete`, return to the session resources. Open the new
   `RABIES_PREPROCESS` resource.

REST equivalent in
[reference/rest-cheatsheet](../reference/rest-cheatsheet.md#container-launches)
with input keys `session`, `bids-resource`, `tr`, `fast_commonspace`.

## Expected output

A `RABIES_PREPROCESS` resource on the session containing:

```
preprocess_outputs/
├── motion_datasink/         # motion parameters per-subject
├── commonspace_datasink/    # subject-to-template registrations
├── confound_datasink/       # nuisance regressors
├── analysis_datasink/       # quality plots
└── rabies_preprocess.csv    # job manifest
```

Plus an HTML QC report at the top of the directory tree.

## Verify

Open the HTML QC report and one motion / confound output. The report
should match the subject and session you launched, and the directory
names should match the RABIES stage.

## If it does not work

- **Failure during BIDS staging**: open the session's `BIDS` resource
  and confirm the layout matches the expected tree above. Missing
  anatomical or functional NIfTIs at the right path is the most common
  cause.
- **RABIES error mid-run**: command history → stderr. CPU-only on a
  large dataset times out; either pick the anatomy-only dataset, or set
  `fast_commonspace=true`.
- **Output resource is sparse**: the run completed but the pipeline
  failed to produce real outputs. Read stderr before assuming the
  biological result is meaningful.

## Demo strategy (45-min slot)

| Minutes | Activity |
|---|---|
| 0–5 | Why preclinical needs different pipelines |
| 5–8 | Show the BIDS resource on a sample session |
| 8–12 | Launch RABIES with `fast_commonspace=true` |
| 12–25 | While it runs: cover RABIES architecture vs fMRIPrep |
| 25–35 | Inspect outputs from a pre-run job: registration overlays, motion plots |
| 35–45 | Q&A; point at confound_correction and analysis stages |

## Talking points

- BIDS in XNAT is the bridge: existing BIDS plugins (`dcm2bids`,
  `bids-tree-builder`) build the input automatically from raw scans.
- RABIES outputs feed naturally into stages 2 (confound correction) and
  3 (analysis) — same wrapper pattern, different image entrypoint.
- The same wrapper template covers AIDAmri, SAMRI, Biomedisa, and other
  BIDS- or directory-based preclinical tools.

## Alternative preclinical containers

- **AIDAmri** — Aswendt Lab mouse multi-modal pipeline.
- **SAMRI** — small-animal MRI with Bruker raw input and NiPype.
- **Biomedisa** — interactive smart-interpolation segmentation.

See [TUTORIAL_CONTAINERS.md](https://github.com/xnatworks/xnat_monailabel_container/blob/main/TUTORIAL_CONTAINERS.md).
