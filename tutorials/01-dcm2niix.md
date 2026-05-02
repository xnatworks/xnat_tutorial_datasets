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
reference and license. The fallback tutorial subset is mirrored in this
repo under `datasets/xnat-tutorial/tcia_dicom_intro/archive.zip`.

## Beginner Guidance

What this tutorial does: starts with one scan's raw DICOM files and runs a
scan-level container that writes derived NIfTI files back to the same scan.

What to look for before launch: the session has a scan table, the chosen scan
has a `DICOM` resource, and the project has the `dcm2niix-scan` wrapper
enabled.

How to know it worked: the scan gains a new `NIFTI` resource with one
`*.nii.gz` image and one `*.json` sidecar. The source `DICOM` resource should
still be present; conversion adds outputs, it does not replace the raw data.

What to check first if it fails: open Container Service command history,
select the failed `dcm2niix` run, and read stderr. Then return to the scan and
confirm the `DICOM` resource contains image DICOM files, not an empty or
non-image folder.

## Setup

1. Upload the DICOM archive to a project on your XNAT.
2. Confirm the `dcm2niix` command is enabled at site + project level
   (Admin → Plugin Settings → Container Service).

## Run via UI

1. Navigate to the session.
2. Right-click the scan → **Run Container** → `dcm2niix-scan`.
3. Accept defaults; click **Run**.
4. Open command history from the project or site navigation.
5. Wait until the run status is `Complete`.
6. Return to the scan and open the new `NIFTI` resource.

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
[Tutorial 04 — MONAI Bundle segmentation](04-monai-bundle-segmentation.md).

## Talking points

- dcm2niix is the de-facto standard for DICOM→NIfTI in research; ships in
  every BIDS pipeline.
- The scan-level `NIFTI` resource is the conventional output location;
  many downstream containers (MONAI, BIDS pipelines) auto-detect it.
- For corrupted or aggressively-anonymized DICOMs, the
  `xnatworks/monai-bundle-nifti` container in Tutorial 04 has a
  pydicom-cleanup fallback baked in.
