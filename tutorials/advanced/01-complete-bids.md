# Complete BIDS workflow

**Level:** Advanced · **Time:** 20–45 min · **Recommended dataset:** `bidscoin_dicom_to_bids`
(fallback: `openneuro_flanker_bids` for the BIDS-input portion only;
alternate DICOM source: `reproin_dicom_to_bids`)

End-to-end DICOM → BIDS → BIDS-app on real data, building on the
intermediate BIDS lessons. Run after
[intermediate/04-dicom-to-bids](../intermediate/04-dicom-to-bids.md) and
[intermediate/06-mriqc-assessor](../intermediate/06-mriqc-assessor.md).

The big idea: BIDS in XNAT is **layered**. Raw DICOM stays on scan
resources. Conversion produces scan-level NIfTI / sidecars.
Materialisation stages those into a session-level BIDS tree. BIDS apps
read the tree through a setup helper and write outputs back as
assessors.

## Preconditions

- XNAT with Container Service installed.
- Tutorial loader configured against
  `https://raw.githubusercontent.com/xnatworks/xnat_tutorial_datasets/main`.
- These commands installed and project-enabled:
  - `bids-mapping-generator` (`xnatworks/bids-mapping-generator:1.7.4`)
  - `dcm2bids-session-v17` (`xnatworks/dcm2bids-session:2.17`)
  - `bids-materialize` (uses `xnatworks/xnat2bids-setup:1.8`)
  - `bids-validator-v2` (`bids/validator:2.5.6`)
  - `bids-mriqc-assessor` (`nipreps/mriqc:24.0.2`,
    wrapup `xnatworks/bids-assessor-wrapup:2.7`)
- Optional heavy apps installed only on prepared sites:
  `fmriprep-session-assessor` (`nipreps/fmriprep:25.2.5`),
  `qsiprep-session-assessor` (`pennlinc/qsiprep:1.1.1`).
- FreeSurfer-dependent workflows (fMRIPrep, QSIPrep) require
  `LICENSES/fs_license.txt` on the project. The tutorial does **not**
  bundle this — each site supplies its own. See
  [reference/rest-cheatsheet](../reference/rest-cheatsheet.md#freesurfer-license-fmriprep--qsiprep).

## 1. Dataset preparation

1. Load `bidscoin_dicom_to_bids` into project `XNAT_TUTORIAL_BIDSCOIN`.
2. Open the project. Confirm one subject, one MR session, **eleven**
   scans:

   | Scan | Role | Files |
   |---|---|---:|
   | `1` | localiser | 3 |
   | `2` | scout | 128 |
   | `7` | T1w MPRAGE | 192 |
   | `47` / `48` | functional SBRef / BOLD | 1 / 10 |
   | `49` / `50` | fieldmap magnitude / phase | 128 / 64 |
   | `59` / `60` | multi-echo SBRef / BOLD | 3 / 30 |
   | `61` / `62` | multi-echo fieldmap mag / phase | 102 / 51 |

3. Confirm the project has tutorial provenance under project resources.
4. Confirm container setup reports mapping, conversion, materialise, and
   MRIQC commands as installed and project-enabled.

If the loader says the session is in prearchive, archive it from
[intro/03-dicom-import-archive](../intro/03-dicom-import-archive.md)
before continuing.

## 2. Inspect source data

Open the DICOM headers on scans `7`, `48`, `49`, `60`, `62`. Connect each
`ProtocolName` / `SeriesDescription` to a BIDS target (`anat`, `func`,
`fmap`). The localiser and scout illustrate scans that should not become
scientific BIDS inputs.

## 3. Generate the BIDS mapping

1. Launch `bids-mapping-generator` from the session **Actions** menu.
2. Save the mapping to project configuration. Wait for `Complete`.
3. Open the mapping resource. Confirm:
   - Scan `7` → anatomical T1w.
   - Scans `48` / `60` → functional BOLD.
   - Scans `47` / `59` → SBRef partners.
   - Scans `49`/`50`/`61`/`62` → fieldmap pairs.
   - Localiser / scout excluded.

The upstream BIDScoin tutorial asks learners to edit task labels into
study-specific names (`task-reward`, `task-stop`). The automated mapping
cannot infer these from scanner protocols alone. Either edit before
running conversion, or accept generic labels for the tutorial.

## 4. Convert DICOM to BIDS resources

1. Launch `dcm2bids-session-v17`. Set:
   - `skip unusable = true`
   - `overwrite = false` (use `true` only when intentionally rerunning
     after deleting prior outputs).
   - `series_description` as the mapping field, unless the project
     mapping says otherwise.
2. Open command history. Watch stdout / stderr.
3. Open each science scan and confirm new NIfTI + BIDS sidecar
   resources. Localiser / scout scans stay raw DICOM-only.

`dcm2bids-session:2.17` preserves echo labels for multi-echo BOLD and
converts Siemens GRE fieldmap pairs into `magnitude1`, `magnitude2`,
`phasediff`.

## 5. Materialise the BIDS resource

1. Launch `bids-materialize`. Confirm the wrapper uses
   `xnatworks/xnat2bids-setup:1.8:xnat2bids`.
2. Wait for `Complete`. Open the session resources.
3. Open the new session-level `BIDS` resource. Confirm
   `dataset_description.json` plus `sub-*/ses-*/{anat,func,fmap}/`.

## 6. Validate

Run [intermediate/03-bids-validation](../intermediate/03-bids-validation.md)
against this `BIDS` resource. Stop here if the validator reports errors —
fix the mapping before running heavy BIDS apps.

## 7. MRIQC

Run [intermediate/06-mriqc-assessor](../intermediate/06-mriqc-assessor.md)
against the same session.

## 8. Optional — fMRIPrep with your own FreeSurfer license

The fMRIPrep wrapper expects exactly:

```
LICENSES/fs_license.txt
```

Add it through the UI (Project → Resources → create `LICENSES` →
upload `fs_license.txt`) or
[via REST](../reference/rest-cheatsheet.md#freesurfer-license-fmriprep--qsiprep).

Then:

1. Confirm the session has a materialised `BIDS` resource and BIDS
   validation passes (or the errors are understood).
2. Launch `fmriprep-session-assessor` with conservative flags, e.g.
   `--participant-label sub-01 --output-spaces MNI152NLin2009cAsym`.
3. Watch setup → fMRIPrep → wrapup containers.
4. Open the resulting fMRIPrep assessor and `DATA` resource.

## Discussion prompt

Ask the learner to map four artefact classes onto the project:

- Acquisition: scan-level `DICOM` resources.
- Conversion: scan-level `NIFTI` + BIDS sidecars.
- Staged BIDS input: session-level `BIDS` resource.
- Analysis output: MRIQC assessor `DATA` resource.

## Troubleshooting

- **dcm2niix fails on the `behavioural` folder** (BIDScoin only): keep
  `skip unusable = true`. Do not delete imaging scans to "fix" this.
- **dcm2niix fails on scan `16` of `reproin_dicom_to_bids`**: reset the
  project, re-prepare the dataset, confirm scan `16` has 4 DICOM files,
  rerun. The Python stack trace alone hides the actual dcm2niix error —
  read stderr from command history.
- **Naming collision** during conversion: edit the mapping to add `echo`,
  `run`, `acq`, or `dir`. Do not rerun with `overwrite = true` until the
  collision is resolved.
- **MRIQC cannot see BIDS**: confirm the wrapper uses the
  `xnat2bids-setup:1.8` setup command, not raw scan folders.
- **Session imports but does not archive**: check the prearchive; archive
  manually if auto-archive is off.

## Reference

- [BIDScoin tutorial](https://bidscoin.readthedocs.io/en/stable/tutorial.html)
- [BIDSconvertR sequence-map example](https://bidsconvertr.github.io/tutorial/)
- [Container Service setup commands](https://wiki.xnat.org/container-service/setup-commands)
- [Viewing Command History and Logs](https://wiki.xnat.org/container-service/viewing-command-history-and-logs)
- [HeuDiConv ReproIn](https://heudiconv.readthedocs.io/en/latest/reproin.html)
