# Tutorial 07 — RABIES rodent fMRI preprocessing

The preclinical anchor: a published, BIDS-driven preprocessing pipeline
purpose-built for **rodent** fMRI/sMRI. Distinct from the human-derived
pipelines (fMRIPrep, qsiprep) — RABIES uses rodent atlases, anatomical
priors, and motion parameters tuned for the rodent brain.

## Container

| Field | Value |
|---|---|
| Image | `ghcr.io/cobralab/rabies:0.6.0` |
| Source | https://github.com/CoBrALab/RABIES |
| Reference | Desrosiers-Grégoire et al. *Nat Commun* 2024 ([doi:10.1038/s41467-024-50826-8](https://www.nature.com/articles/s41467-024-50826-8)) |
| Walltime (1 subject) | ~60 min default; ~10–15 min with `fast_commonspace=true` |
| GPU | optional (helps registration; CPU also works) |

## Pipeline stages

| Stage | Subcommand | Wrapped? |
|---|---|---|
| Preprocess | `rabies preprocess` | ✅ — this tutorial |
| Confound correction | `rabies confound_correction` | TODO |
| Analysis (FC, QC) | `rabies analysis` | TODO |

## Datasets

See [`sources.yml`](sources.yml) entries:

- `07-rabies-rodent-fmri` — full BIDS rodent rs-fMRI (anatomy + functional)
  for an end-to-end demo.
- `07-rabies-rodent-fmri-anatomy-only` — mouse anatomical-only mirror;
  use as a sanity check for brain extraction / atlas registration.

## Expected BIDS layout

RABIES expects:

```
${BIDS_ROOT}/
└── sub-<id>/
    └── ses-<id>/                              # session optional
        ├── anat/sub-<id>_T2w.nii.gz           # or T1w
        └── func/sub-<id>_task-rest_bold.nii.gz
```

Each NIfTI needs a sidecar `*.json` with at minimum `RepetitionTime`.

XNAT BIDS pipelines (`bids-tree-builder`, `dcm2bids-session-v16`)
generate this structure from raw scans — run them first if a session
doesn't yet have a BIDS resource.

## Beginner Guidance

What this tutorial does: shows that a preclinical BIDS app can run from XNAT
when the session already has a valid BIDS resource. RABIES is not launched
directly on raw scan folders; it consumes the BIDS directory tree.

What to look for before launch: the session has a `BIDS` resource with
`sub-*` folders, an anatomical file, a functional BOLD file, and JSON sidecars
with timing metadata such as `RepetitionTime`.

How to know it worked: the command reaches `Complete` and the session gains a
`RABIES_PREPROCESS` resource with motion, common-space, confound, and QC/report
outputs.

What to check first if it fails: open command history and check whether failure
happened during BIDS input staging or during RABIES itself. If staging failed,
inspect the session's `BIDS` resource first; if RABIES failed, inspect the
RABIES stderr and confirm required anatomical/functional files exist.

## XNAT setup

1. Pull the image (one-time, large):

   ```bash
   docker pull ghcr.io/cobralab/rabies:0.6.0
   ```

2. Register the command (`command.json` lives in the
   [xnat_monailabel_container repo](https://github.com/xnatworks/xnat_monailabel_container/blob/main/preclinical/rabies/command.json)):

   ```bash
   SESS=$(curl -s -u ${XNAT_USER}:${XNAT_PASS} ${XNAT_HOST}/data/JSESSION)
   curl -b "JSESSIONID=$SESS" -X POST -H 'Content-Type: application/json' \
     -d @command.json \
     ${XNAT_HOST}/xapi/commands
   ```

3. Enable the wrapper at site + project level.

## Run via UI

1. Open a session that has a `BIDS` resource attached.
2. Click **Run Containers** at the session level → **RABIES preprocess (BIDS resource)**.
3. Pick BIDS resource, set TR, set `fast_commonspace=true` for live demo.
4. Click **Run** — outputs land as a `RABIES_PREPROCESS` resource.
5. Open command history and wait for `Complete`.
6. Return to the session resources and open `RABIES_PREPROCESS`.

## Run via REST

```bash
SESS=$(curl -s -u ${XNAT_USER}:${XNAT_PASS} ${XNAT_HOST}/data/JSESSION)

curl -b "JSESSIONID=$SESS" -X POST -H 'Content-Type: application/json' \
  -d '{
        "session":"/archive/projects/${PROJECT}/experiments/${SESSION}",
        "bids-resource":"/archive/projects/${PROJECT}/experiments/${SESSION}/resources/BIDS",
        "tr":"1.2",
        "fast_commonspace":"true"
      }' \
  ${XNAT_HOST}/xapi/projects/${PROJECT}/wrappers/${WRAPPER_ID}/launch
```

## Expected output

`RABIES_PREPROCESS` resource on the session, containing:

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

Open the HTML QC report and one motion/confound output. The report should
correspond to the subject/session you launched, and the output folder names
should match the RABIES stage. If the output resource is present but sparse,
read stderr before assuming the biological result is meaningful.

## Demo strategy (45-min tutorial slot)

| Minutes | Activity |
|---|---|
| 0–5 | Why preclinical needs different pipelines (physical scale, anatomy, atlas, motion) |
| 5–8 | Show BIDS resource on a sample session in XNAT |
| 8–12 | Launch RABIES with `fast_commonspace=true` from UI |
| 12–25 | While it runs: cover RABIES architecture, contrast with fMRIPrep |
| 25–35 | Inspect outputs from a *pre-run* job: registration overlays, motion plots |
| 35–45 | Q&A, point at confound_correction + analysis stages |

## Talking points

- BIDS in XNAT is the bridge: existing BIDS plugins (dcm2bids,
  bids-tree-builder) build the input automatically from raw scans.
- RABIES outputs feed naturally into stages 2 (confound correction) and 3
  (analysis) — same wrapper pattern, different image entrypoint.
- Reproducibility: pinned image tag + container labels + XNAT provenance
  audit trail = full FAIR pipeline.
- Extensible: same wrapper template covers AIDAmri, SAMRI, Biomedisa,
  any other BIDS- or directory-based preclinical tool.

## Alternative preclinical containers

If RABIES is too heavy for your audience, swap in:

- **AIDAmri** — Aswendt Lab's mouse multi-modal pipeline (atlas
  registration, DTI, rs-fMRI seed FC).
- **SAMRI** — small-animal MRI with Bruker raw input and NiPype.
- **Biomedisa** — interactive smart-interpolation segmentation,
  microCT/microMRI.

See [TUTORIAL_CONTAINERS.md](https://github.com/xnatworks/xnat_monailabel_container/blob/main/TUTORIAL_CONTAINERS.md)
in the source repo for full notes on each.
