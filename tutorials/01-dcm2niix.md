# Tutorial 01 — DICOM → NIfTI with dcm2niix

The foundation step for almost every downstream pipeline: convert a DICOM
series into a single NIfTI volume. Demonstrates the simplest possible
XNAT Container Service flow.

## Container

| Field | Value |
|---|---|
| Image | `xnatworks/dcm2niix:2.0` |
| Tool | dcm2niix v1.0.20250506+ (with turboJPEG, OpenJPEG, CharLS) |
| XNAT command | `dcm2niix` |
| Wrapper context | scan |
| Walltime | < 1 minute for a typical 200-slice CT |
| GPU | not required |

## Dataset

See [`sources.yml`](sources.yml) entry `01-dcm2niix` for the upstream
reference and license. The fallback workshop subset is mirrored in this
repo under `datasets/xnat-tutorial/tcia_dicom_intro/archive.zip`.

## Setup

1. Upload the DICOM archive to a project on your XNAT.
2. Confirm the `dcm2niix` command is enabled at site + project level
   (Admin → Plugin Settings → Container Service).

## Run via UI

1. Navigate to the session.
2. Right-click the scan → **Run Container** → `dcm2niix-scan`.
3. Accept defaults; click **Run**.

## Run via REST

```bash
SESS=$(curl -s -u ${XNAT_USER}:${XNAT_PASS} ${XNAT_HOST}/data/JSESSION)

curl -b "JSESSIONID=$SESS" -X POST -H 'Content-Type: application/json' \
  -d '{"scan":"/archive/projects/${PROJECT}/experiments/${SESSION}/scans/${SCAN}"}' \
  ${XNAT_HOST}/xapi/projects/${PROJECT}/wrappers/${WRAPPER_ID}/launch
```

Look up `${WRAPPER_ID}` from `GET /xapi/commands` (filter where
`name == "dcm2niix"` and the wrapper context is `xnat:imageScanData`).

## Expected output

A new resource appears on the scan called `NIFTI` containing:

- `<series>.nii.gz` — the converted volume
- `<series>.json` — BIDS-style sidecar (acquisition parameters)

## Verify

```bash
curl -s -u ${XNAT_USER}:${XNAT_PASS} \
  "${XNAT_HOST}/data/projects/${PROJECT}/experiments/${SESSION}/scans/${SCAN}/resources/NIFTI/files?format=json" \
  | python3 -c 'import json,sys; [print(f["Name"], f["Size"]) for f in json.load(sys.stdin)["ResultSet"]["Result"]]'
```

You should see one `*.nii.gz` and one `*.json`. The NIfTI is the input for
[Tutorial 02 — MONAI Bundle segmentation](02-monai-bundle-segmentation.md).

## Talking points

- dcm2niix is the de-facto standard for DICOM→NIfTI in research; ships in
  every BIDS pipeline.
- The scan-level `NIFTI` resource is the conventional output location;
  many downstream containers (MONAI, BIDS pipelines) auto-detect it.
- For corrupted or aggressively-anonymized DICOMs, the
  `xnatworks/monai-bundle-nifti` container in Tutorial 02 has a
  pydicom-cleanup fallback baked in.
