# Container launch basics

**Level:** Intermediate · **Time:** 15 min · **Recommended dataset:** `openneuro_flanker_bids`
(also works with `tcia_dicom_intro`)

## Goal

Understand that the **launch context** (where you click "Run Container")
determines what XNAT mounts inside the container, and find the output
resource the run produces.

## Preconditions

- Container Service installed.
- At least one wrapper enabled at the project. The
  [BIDS validator](03-bids-validation.md) or
  [dcm2niix](01-dcm2niix.md) wrappers are the simplest.

## Walkthrough

1. Open the project, session, scan, or resource the wrapper is scoped for.
   - Scan-context wrappers (e.g. `dcm2niix-scan`): launch from the scan
     row.
   - Session-context wrappers (e.g. `bids-validator-v2`): launch from the
     session **Actions** box.
   - Project-context wrappers: launch from the project page.
2. Open **Run Containers** in the right context. Pick the command.
3. Read the launch dialog. Confirm the input path is what you expected
   (the scan or resource you opened).
4. Submit.
5. Open command history. Watch the run move through `Created` →
   `Running` → `Complete`.
6. Click the completed run to read stdout / stderr.
7. Return to the source object. Open the new output resource.

## Expected result

You can describe the relationship between the **launch context** and the
**input mount path** the container sees, and find the output resource
that came from your run.

## Verify

The source object now has a new output resource that did not exist before
the run.

## If it does not work

- **Wrapper not visible**: it is not enabled at the right scope. Site
  admins enable wrappers in **Administer → Plugin Settings → Container
  Service**; project owners enable them per-project.
- **Container fails immediately**: read stderr in command history. The
  most common cause is launching from the wrong scope (a session-context
  wrapper launched from a scan, or vice versa).

## Reference

[Launching Containers from Commands](https://wiki.xnat.org/container-service/launching-containers-from-commands)
