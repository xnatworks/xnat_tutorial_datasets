# Plugin loader — instructor / admin view

**Level:** Admin · **Time:** 15 min · **Recommended dataset:** any from
[../datasets/](../datasets/)

## Goal

Pre-load tutorial datasets in bulk before a workshop, verify they
imported cleanly, and know how to reset a tutorial project quickly
between sessions.

## Preconditions

- Tutorial dataset plugin installed.
- Site administrator account.
- Network access to the datasets — primary source first, then the raw
  GitHub fallback at
  `https://raw.githubusercontent.com/xnatworks/xnat_tutorial_datasets/main`.

## Bulk pre-load via REST

```bash
export XNAT_HOST=https://your.xnat.example
export XNAT_USER=admin
export XNAT_PASS=admin

for dataset in tcia_dicom_intro tcia_pseudo_phi_deid \
               openneuro_flanker_bids openneuro_ds002551_metadata \
               niivue_demo_images bidscoin_dicom_to_bids; do
  curl -u "${XNAT_USER}:${XNAT_PASS}" -X POST \
    "${XNAT_HOST}/xapi/tutorials/datasets/${dataset}/prepare?projectId=XNAT_TUTORIAL_${dataset^^}"
done
```

Each dataset's plugin id and default project are listed in its
[reference card](../datasets/).

## Verify each load

For DICOM datasets:

1. Open the project. Confirm subjects / sessions / scans match the
   reference card.
2. Open the prearchive. If a row is stuck, archive it manually.
3. Open one DICOM resource and confirm it has files.

For BIDS datasets:

1. Open the project. Confirm a session-level or project-level `BIDS`
   resource exists.
2. Open `dataset_description.json`.
3. Confirm subject directories under the resource.

For NiiVue / metadata-only datasets:

1. Open the project resources panel.
2. Confirm the expected count of files (~70 NIfTIs for `niivue_demo_images`,
   3 metadata files for `openneuro_ds002551_metadata`).

## Reset a tutorial project

For a clean re-run between sessions:

1. Open the project. **Manage → Delete Project**. Confirm.
2. Re-prepare via the loader. The plugin recreates the project with
   tutorial provenance metadata.

For a softer reset that keeps the project but clears imported data:

1. Delete subjects from the project page.
2. Open the prearchive and clear any stuck rows scoped to the project.
3. Re-prepare.

## Known fallback caveats

- Group-level datasets (FitLins Flanker, larger MSD tasks, TumSeg
  micro-CT) are **not** mirrored as raw GitHub files. They live in the
  source catalog at `datasets/grouplevel/sources.yml`. For a fallback,
  publish them as GitHub release assets or in object storage and point
  loader source URLs at those endpoints.
- The MSD Hippocampus archive **is** mirrored as a normal raw file
  (~30 MB). Use that path for the nnU-Net dataset lesson.

## Verify with checksums

```bash
cd /path/to/xnat_tutorial_datasets
shasum -a 256 -c SHA256SUMS
```

Run after refreshing or reseeding the dataset mirror to confirm bit-for-
bit identical files.

## Reference

- [`../README.md`](../README.md) — canonical lesson index.
- `../sources.yml` — authoritative dataset source list.
- `../../README.md` — dataset inventory (this repo's root).
