# REST Cheatsheet

Quick `curl` recipes used by the intermediate, advanced, and admin lessons.
Set these once in your shell:

```bash
export XNAT_HOST=https://your.xnat.example
export XNAT_USER=admin
export XNAT_PASS=admin
export PROJECT=XNAT_TUTORIAL_DICOM
```

## Sessions and authentication

```bash
# Get a JSESSION token (avoids sending basic auth on every request)
SESS=$(curl -s -u ${XNAT_USER}:${XNAT_PASS} ${XNAT_HOST}/data/JSESSION)
```

## Tutorial dataset loader

```bash
# Stage source files only
curl -u ${XNAT_USER}:${XNAT_PASS} -X POST \
  ${XNAT_HOST}/xapi/tutorials/datasets/${DATASET_ID}/download

# Stage + create the project + import (typical)
curl -u ${XNAT_USER}:${XNAT_PASS} -X POST \
  "${XNAT_HOST}/xapi/tutorials/datasets/${DATASET_ID}/prepare?projectId=${PROJECT}"
```

`${DATASET_ID}` is the plugin id from each dataset reference card under
[`../datasets/`](../datasets/).

## Resource files

```bash
# List files in a resource
curl -s -u ${XNAT_USER}:${XNAT_PASS} \
  "${XNAT_HOST}/data/projects/${PROJECT}/experiments/${SESSION}/scans/${SCAN}/resources/NIFTI/files?format=json"

# Upload a single file into a project resource (creates the resource if missing)
curl -u ${XNAT_USER}:${XNAT_PASS} -X PUT \
  --data-binary @/path/to/file \
  "${XNAT_HOST}/data/projects/${PROJECT}/resources/${RESOURCE_LABEL}/files/${FILE_NAME}?overwrite=true"
```

## Container launches

```bash
# Look up wrapper IDs
curl -s -u ${XNAT_USER}:${XNAT_PASS} ${XNAT_HOST}/xapi/commands

# Launch a scan-context wrapper
curl -b "JSESSIONID=$SESS" -X POST -H 'Content-Type: application/json' \
  -d '{"scan":"/archive/projects/'${PROJECT}'/experiments/'${SESSION}'/scans/'${SCAN}'"}' \
  ${XNAT_HOST}/xapi/projects/${PROJECT}/wrappers/${WRAPPER_ID}/launch

# Launch a session-context wrapper
curl -b "JSESSIONID=$SESS" -X POST -H 'Content-Type: application/json' \
  -d '{"session":"/archive/projects/'${PROJECT}'/experiments/'${SESSION}'"}' \
  ${XNAT_HOST}/xapi/projects/${PROJECT}/wrappers/${WRAPPER_ID}/launch
```

## FreeSurfer license (fMRIPrep / QSIPrep)

```bash
curl -u "${XNAT_USER}:${XNAT_PASS}" -X PUT \
  --data-binary @/path/to/your/fs_license.txt \
  "${XNAT_HOST}/data/projects/${PROJECT}/resources/LICENSES/files/fs_license.txt?overwrite=true"
```

The wrapper expects exactly that path: project resource label `LICENSES`,
file name `fs_license.txt`.

## Variables used across the lessons

| Variable | Meaning |
|---|---|
| `${XNAT_HOST}` | XNAT base URL, no trailing slash. |
| `${XNAT_USER}` / `${XNAT_PASS}` | Account with the rights the lesson needs. |
| `${PROJECT}` | Project ID (not title). |
| `${SESSION}` | Experiment ID or label. |
| `${SCAN}` | Scan ID inside the session. |
| `${WRAPPER_ID}` | Container wrapper id from `GET /xapi/commands`. |
| `${DATASET_ID}` | Tutorial plugin dataset id (e.g. `tcia_dicom_intro`). |
