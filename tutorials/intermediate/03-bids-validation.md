# BIDS validation

**Level:** Intermediate · **Time:** 15 min · **Recommended dataset:** `openneuro_flanker_bids`

## Goal

Run the BIDS validator container against a `BIDS` resource and review the
pass / fail report it stores back as an assessor.

## Preconditions

- Container Service installed.
- The `bids-validator-v2` command enabled at site and project level.
- A project that already has a session-level `BIDS` resource. Use
  [intro/08-bids-as-resource](../intro/08-bids-as-resource.md) if you do
  not have one yet.

## Walkthrough

1. Load `openneuro_flanker_bids` into `XNAT_TUTORIAL_BIDS` (skip if
   already loaded).
2. Open the session and confirm the `BIDS` resource exists.
3. Open the project **Actions** menu and launch **bids-validator-v2**.
4. Confirm the wrapper uses the BIDS setup helper
   (`xnatworks/xnat2bids-setup:1.8:xnat2bids`).
5. Submit the launch. Open command history and wait for both the main
   validator and the wrapup container to complete.
6. Open the generated BIDS validator assessor.
7. Open its `DATA` resource. Review `validation_report.json`,
   `validation_report.txt`, and any HTML report.

## Expected output

- An assessor of type `bids:bidsValidatorRunAssessorData` (or similar)
  attached to the session.
- The assessor records pass / fail status, error count, and warning count.
- The `DATA` resource preserves the raw validator output.

The validator command **exits successfully even when the dataset has
errors**. Pass/fail is recorded on the assessor, not the container exit
code.

## Verify

The session has a BIDS validator assessor with explicit pass / fail and a
non-empty `DATA` resource.

## If it does not work

- Command resolution failure: confirm the wrapup command
  `xnatworks/bids-assessor-wrapup:2.7:bids-assessor-wrapup` is installed
  and that the BIDS assessor schema plugin is loaded.
- Empty `DATA` resource: re-check the wrapup container's logs in command
  history; it is the step that uploads the report.

## Reference

- [Launching Containers from Commands](https://wiki.xnat.org/container-service/launching-containers-from-commands)
- [Viewing Command History and Logs](https://wiki.xnat.org/container-service/viewing-command-history-and-logs)
