# TCIA QIN-PROSTATE-Repeatability — smallest series, auto-selected

## Why this dataset

Same QIN-PROSTATE-Repeatability collection as
[`tcia_dicom_intro`](tcia_dicom_intro.md), but loaded with a different
plugin mode that **auto-selects the smallest series in the collection**
at load time. No hardcoded series UID.

This matters for institutions: site policy may forbid pinning a single
series UID into a tutorial config because the upstream collection can
mutate. Asking the NBIA API for "give me the smallest MR series in this
collection right now" produces a stable workshop demo without a
maintenance burden when TCIA reorganizes a collection.

It's also a teaching point in itself: the same dataset repository can be
queried by **collection** rather than by **series**, and the plugin
shows how XNAT can be a downstream of dynamic public archives.

| | |
|---|---|
| Modality | MR |
| License | CC-BY-4.0 |
| Source | [TCIA QIN-PROSTATE-Repeatability](https://www.cancerimagingarchive.net/collection/qin-prostate-repeatability/) |
| Plugin id | `tcia_collection_smallest` |
| Mode | `nbia_collection` (vs `nbia_series` in `tcia_dicom_intro`) |
| Selection | smallest by FileSize, modality=MR |
| Default project | `XNAT_TUTORIAL_DICOM` |

## Download via the tutorial plugin

**UI** — Admin → **Tutorial Datasets** (`${XNAT_HOST}/xnat-tutorial/datasets.html`).
Find `tcia_collection_smallest` in the list, click **Prepare**, set the project id, submit.

**REST** (admin auth required):

```bash
# stage source files only — leaves them in the plugin staging area
curl -u ${XNAT_USER}:${XNAT_PASS} -X POST \
  ${XNAT_HOST}/xapi/tutorials/datasets/tcia_collection_smallest/download

# stage + create the project + import (typical)
curl -u ${XNAT_USER}:${XNAT_PASS} -X POST \
  "${XNAT_HOST}/xapi/tutorials/datasets/tcia_collection_smallest/prepare?projectId=XNAT_TUTORIAL_DICOM"
```

## What you get in XNAT

Same shape as `tcia_dicom_intro`:

- 1 project, 1 subject, 1 MR session, 1 scan
- Whichever series happened to be smallest in the collection at load
  time

## What to do with it

Identical to `tcia_dicom_intro`. The teaching value here is **not** the
data — it's the **mechanism**.

## Workshop flow (5 minutes)

1. Show the manifest entries side-by-side: `tcia_dicom_intro` (series
   UID pinned) vs `tcia_collection_smallest` (collection-driven).
2. Discuss tradeoffs:
   - Pinned UID: deterministic, but breaks when upstream reorganizes.
   - Collection query: stable name, content can drift.
3. Both modes hit the same NBIA REST endpoints; the plugin handles the
   difference. The XNAT side is identical.

## Talking points

- Tutorials are software — they need maintenance budgets. Choosing
  load mechanics with the right durability vs reproducibility tradeoff
  is part of the design.
- TCIA is one of many DICOM archives accessible via NBIA-style APIs;
  the same plugin pattern supports IDC, CTP-receivable inboxes, etc.
