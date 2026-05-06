# Command enablement

**Level:** Admin · **Time:** 15 min · **Recommended dataset:** any
tutorial project

## Goal

Diagnose whether a missing tutorial command is a **site registration**
problem, a **project enablement** problem, or a **missing resource**
problem — and fix it without touching unrelated configuration.

## Preconditions

- Container Service installed.
- Site administrator account.
- A tutorial project loaded and a known-failing command (e.g. one of the
  BIDS demo commands).

## Walkthrough

1. Open **Administer → Plugin Settings → Container Service →
   Images & Commands**.
2. Confirm the command's Docker image is listed and pulled. If not, pull
   it on the Docker host or install through the registry UI.
3. Confirm the command JSON is registered. If it is missing, install the
   curated command JSON from the source repo
   (`xnatworks/xnat_monailabel_container` for tutorial commands;
   `xnat_tutorial_plugin/containers/manifest.yml` lists pinned tags).
4. Open the command and confirm it has at least one wrapper.
5. Open the project. Open **Project Settings → Container Service →
   Project commands** (path varies by version).
6. Enable the wrapper at project scope.
7. Confirm any required project resources exist:
   - FreeSurfer-dependent workflows: `LICENSES/fs_license.txt`.
   - Setup-helper-based BIDS apps: a session-level `BIDS` resource.
8. Re-launch the command. Read command history if it still fails.

## Expected result

The command resolves and launches when invoked from the right context.
The launch dialog populates with the inputs the wrapper expects.

## Verify

A test launch on a tutorial dataset (e.g. `dcm2niix` against
`tcia_dicom_intro`) reaches `Complete` and writes its output resource.

## If it does not work

Triage by symptom:

| Symptom | Likely cause | Fix |
|---|---|---|
| Wrapper not visible in Actions menu | Project enablement | Enable at project scope. |
| Wrapper visible but launch dialog is blank | Wrapper context mismatch | Confirm you are launching from the right level (scan vs session vs project). |
| Launch fails on input resolution | Required resource missing | Check the command JSON for required resources (e.g. `BIDS`, `LICENSES`). |
| Container starts then fails fast | Image / argument mismatch | Read stderr; confirm pinned image tag matches the command JSON. |
| Wrapup container fails to register an assessor | Schema plugin missing | Confirm the BIDS assessor schema plugin is loaded for BIDS apps. |

## Reference

- [Getting Started with Container Service](https://wiki.xnat.org/container-service/getting-started)
- [Launching Containers from Commands](https://wiki.xnat.org/container-service/launching-containers-from-commands)
- `containers/manifest.yml` in this repo — pinned image tags and command
  JSON checksums.
