# DICOM to NIfTI with dcm2niix

**Level:** Intermediate · **Time:** 15 min · **Recommended dataset:** `tcia_dicom_intro`
(also works with `tcia_mouse_astrocytoma_mri`)

## Goal

Run a scan-level container, watch it complete in command history, and
inspect the new NIfTI resource it writes back to the scan.

| | |
|---|---|
| Image | `xnatworks/dcm2niix:2.0` |
| Wrapper context | scan |
| GPU | not required |
| Walltime | < 1 minute for a typical 200-slice CT |

## Preconditions

- Container Service installed.
- The `dcm2niix` command enabled at site and project level.
- A scan with a `DICOM` resource. Use
  [intro/03-dicom-import-archive](../intro/03-dicom-import-archive.md) if
  you need one.

## Walkthrough

1. Open the session and the scan table.
2. Right-click the scan → **Run Container** → `dcm2niix-scan`.
3. Accept defaults; click **Run**.
4. Open command history (project navigation or site navigation) and wait
   for the run to reach `Complete`.
5. Return to the scan and open the new `NIFTI` resource.

REST equivalent in [reference/rest-cheatsheet](../reference/rest-cheatsheet.md#container-launches);
look up `${WRAPPER_ID}` from `GET /xapi/commands` (filter where
`name == "dcm2niix"`).

## Expected output

A scan-level `NIFTI` resource containing:

- `<series>.nii.gz` — converted volume.
- `<series>.json` — BIDS-style sidecar (acquisition parameters).

The original `DICOM` resource is untouched. Conversion **adds** outputs.

## Verify

```bash
curl -s -u ${XNAT_USER}:${XNAT_PASS} \
  "${XNAT_HOST}/data/projects/${PROJECT}/experiments/${SESSION}/scans/${SCAN}/resources/NIFTI/files?format=json" \
  | python3 -c 'import json,sys; [print(f["Name"], f["Size"]) for f in json.load(sys.stdin)["ResultSet"]["Result"]]'
```

You should see one `*.nii.gz` and one `*.json`.

## If it does not work

- Open command history → failed run → stderr. Most failures are caused by
  a non-image folder (no DICOM image data) or by a project where the
  `dcm2niix` wrapper is not enabled.
- For aggressively-anonymised DICOMs, the
  [MONAI bundle container](../advanced/02-monai-segmentation.md) has a
  pydicom cleanup fallback that produces a converted NIfTI even when
  dcm2niix alone refuses.

## Talking points

- dcm2niix is the de-facto standard for DICOM→NIfTI in research; it ships
  in every BIDS pipeline.
- The scan-level `NIFTI` resource label is the convention many downstream
  containers (MONAI, BIDS pipelines) auto-detect.

Next: [intermediate/05-container-basics](05-container-basics.md) for the
container model in general.
