# HANDOFF — XNAT Tutorial Datasets

## 1. Context Summary

- **Repo:** `https://github.com/xnatworks/xnat_tutorial_datasets`
  (working directory `/Users/james/projects/development/xnat_tutorial_datasets`).
- **Companion repo (not modified this session):**
  `/Users/james/projects/development/xnat_tutorial_plugin` — the plugin
  whose loader UI consumes this repo's `tutorials/sources.yml` and
  mirrored `datasets/`.
- **Hosted XNAT used for testing:** `https://demo.pro.xnatworks.io`
  (= `demo26.pt.xnatworks.io`). Public hosted demo lives at
  `https://demo.pro.xnatworks.io`.

**Original goal:** simplify per-dataset tutorial instructions and
restructure the catalog into pickable lessons by level (intro /
intermediate / advanced / admin) for general XNAT workshops.

**Why:** the prior catalog had three overlapping layers (numbered
walkthroughs, dataset pages, and a `simple-walkthroughs.md` omnibus).
A single canonical lesson tree by level makes "pull a 30-minute
tutorial that fits this audience" tractable.

## 2. Current State (HONEST ASSESSMENT)

### Features — Verified Working ✅

- **Lesson tree restructure (PR #12, merged).** Verified by browsing
  files at `tutorials/{intro,intermediate,advanced,admin,reference}/`
  and confirming every level table in root `README.md` resolves.
- **Hosted demo URL points at the live XNAT.**
  `https://demo.pro.xnatworks.io` returns HTTP 200 (redirect to
  XNAT login). Verified with `curl -I` this session.
- **Plugin loader REST endpoints reachable on demo.pro.** Confirmed
  `/data/JSESSION`, `/xapi/users/username`, `/xapi/commands`,
  `/xapi/projects/Prostate-AEC/...` all respond as documented.
- **TotalSegmentator wrapper end-to-end on demo.pro.** Verified live
  this session:
  - Workflow id `7951`, container id `395`.
  - Project `Prostate-AEC`, subject `Prostate-AEC-044`, session
    `XNAT_E00043`, scan `2` (pelvis CT, 166 DICOM files).
  - Container reached `Complete` after ~10 min image pull + ~3 min
    inference on a `g4dn.xlarge` GPU node.
  - Outputs landed: scan-level `TotalSegmentator-Output` resource (1
    file, RTStruct DICOM, ~6 MB) **and** session-level
    `icr:roiCollectionData` assessor `RoiCollection_sTTq83QI_2nA7pNW7j9C`
    (label `TS-RTStruct-PC-XNAT_E00043--scan-2-20260506_203929`).
- **TCIA Prostate-AEC dataset mirror.** Downloaded via REST (`83 MB
  ZIP`, 166 DICOM files) and committed to PR #14 at
  `datasets/xnat-tutorial/tcia_prostate_aec/archive.zip`. Verified
  contents with `unzip -l`.
- **Command JSON memory/CPU patch.** Local
  `/Users/james/projects/development/total-segmentator/command.json`
  now has `reserve-memory: 14000`, `limit-memory: 15000`,
  `limit-cpu: 4`. Same values pushed to the registered command
  (id 37) on demo.pro via `POST /xapi/commands/37`. Without this
  patch the container sits in `pending` forever (16 GB RAM /
  4 vCPU node can't satisfy 28000/32000/8).

### Features — UI Only / Partial 🔧

- **PR #14 (`cleanup/remove-monai-bundle`)** — `MERGEABLE`/`CLEAN` but
  **not yet merged**. Contains: MONAI lesson removal, advanced/
  renumbering, new `advanced/05-totalsegmentator.md` (with OHIF
  workflow + ROI Collection assessor description), Prostate-AEC
  dataset, dataset card, manifest entry, DATASETS.md inventory row.
- **README hosted-demo URL on PR #14 branch is uncommitted.** Local
  edit done this session sets the URL to `https://demo.pro.xnatworks.io`
  in two places (README.md lines 8 and 92), plus the same constant in
  `slides/build_tutorial_slides.py`. `SHA256SUMS` rehashed. **Not yet
  `git add`'d / committed / pushed.** When the new session runs
  `git status` it will see this drift.
- **Slide decks generated, not committed.**
  `slides/xnat-tutorials.pptx` (34 slides, lesson catalog) and
  `slides/xnat-workflow-capabilities.pptx` (7 slides, pipeline stages
  reworded for an XNAT instance audience) plus their generator
  scripts and a `slides/README.md`. Files exist on disk; the user has
  not decided whether to commit them, gitignore them, or use a
  release-asset path.

### Features — Not Implemented ❌

- **Plugin verdicts never resolved.** Earlier I posted a list (FitLins
  wrapper, nnU-Net inference container, QSIPrep, TotalSegmentator
  fork target, Group-Level loader UI path, `xnat_workbench` install
  assumption) and asked the user to mark each *live / hide / drop*.
  The user has not replied. Lessons currently reference all of them as
  if live. **Action needed before public release.**
- **`xnat_workbench` plugin install on demo.pro is assumed but not
  verified.** `intro/09-niivue-overlays.md` and the TS lesson's NIfTI
  section both rely on **Open with Workbench**. I have not personally
  confirmed the plugin is installed on demo.pro / the hosted demo.
- **OHIF auto-promotion of RTStruct → ROI Collection on other
  XNATs.** It works on demo.pro (verified). Whether it works
  elsewhere depends on site config — the lesson notes this caveat,
  but no admin lesson explains how to set it up.
- **Screenshots in tutorials.** The user asked for browser-driven
  screenshots; the Claude-in-Chrome extension was returning
  `crypto is not defined` and never came up. None captured.
- **Stale entries in `tutorials/sources.yml`.** Old slugs like
  `01-dcm2niix`, `04-monai-bundle-segmentation`,
  `05-totalsegmentator-vs-monai` are still defined in the manifest
  even though those lessons are deleted. Only the new
  `advanced/05-totalsegmentator` slug was added; the orphans were
  left in place. Cleanup pending.
- **Per-instance branding/styling on the slide decks.** Decks are
  intentionally minimal (title + bullets + speaker notes) so the user
  can paste into an existing template. No template has been applied.
- **Other Prostate-AEC subjects.** Only `Prostate-AEC-044` was
  packaged. If the workshop wants several subjects, more downloads
  are needed (and each is ~80 MB).

## 3. Next Steps

In order:

1. **Decide on the uncommitted local changes.**
   ```bash
   cd /Users/james/projects/development/xnat_tutorial_datasets
   git status -sb
   git diff README.md SHA256SUMS
   ```
   Either commit + push the URL fix to PR #14, or revert it. If
   committing, also decide whether to fold in `slides/` or leave as
   separate work.
2. **Resolve plugin verdicts.** Reread the chat log around the
   "FitLins / nnU-Net / QSIPrep / Group-Level loader / xnat_workbench"
   list. Get the user to mark each *live / hide / drop*. Then prune
   lessons accordingly in a follow-up commit.
3. **Verify Workbench plugin on the public host.** Hit
   `https://demo.pro.xnatworks.io` and check
   `xapi/plugins` (or browse a NIfTI resource and look for **Open
   with Workbench**). Update `intro/09` if the plugin isn't installed.
4. **Clean stale `tutorials/sources.yml` entries.** Delete the old
   slug entries that no longer have backing lessons (`01-dcm2niix`,
   `04-monai-bundle-segmentation`, etc.). Audit which `tutorial:` slugs
   still match files under `tutorials/`.
5. **Merge PR #14** once 1–4 are addressed.
6. **(Optional) Browser screenshots for tutorials.** Need the
   Claude-in-Chrome extension reloaded so the `crypto` errors stop.
   Then capture, e.g., the OHIF Contours panel showing the TS ROI
   Collection on demo.pro for `intro/05` and `advanced/05`.

## 4. Key Information

- **PR #14 branch:** `cleanup/remove-monai-bundle`. URL:
  `https://github.com/xnatworks/xnat_tutorial_datasets/pull/14`.
  Squashed to one commit `03b745c` on top of merged main
  (`9b54fe4`/`20649ed`).
- **Earlier orphan branch on origin:** `refactor/tutorial-restructure`.
  Was re-created on origin after PR #12 merged (a `git push`
  side-effect). Safe to delete on origin once confirmed nobody is
  using it; the URL fix it carried is already on main as PR #13.
- **Local creds for demo.pro:** stored in conversation only — not in
  any committed file. Username `jamesAdmin`, password sent inline by
  the user. Do **not** add to `~/.netrc` or repo config.
- **demo.pro auth gotcha:** the JSESSION endpoint returns an HTML
  page when basic auth fails (bug-or-feature) — easy to mistake for
  a valid token. After `curl -u user:pass /data/JSESSION`, validate
  with `curl -b cookie /xapi/users/username` returning HTTP 200.
- **`xnat_map_plugin` location:** `/Users/james/projects/development/xnat_map_plugin`
  (Leaflet world map of XNAT installations). The user briefly asked
  me to "find the map plugin" but didn't say what to do with it.
- **PowerPoint generator quirk:** the deck has `~$xnat-tutorials.pptx`
  lockfiles when the user has the deck open in Keynote / PowerPoint;
  these are local-only and should be `.gitignore`d if `slides/` is
  committed.
- **Memory/CPU constraint table for demo.pro Swarm nodes:**
  `g4dn.xlarge` has **4 vCPUs** and **16 GB RAM**. Container
  `reserve-memory` ≤ 14000, `limit-memory` ≤ 15000, `limit-cpu` ≤ 4
  to land on those nodes.

## 5. Test Data

On `https://demo.pro.xnatworks.io`:

- **Project `Prostate-AEC`** — 110 CT sessions, all RX-simulation,
  all from TCIA (UID prefix `1.3.6.1.4.1.14519.5.2.1...`).
- **Verified TS-compatible scan:** subject `Prostate-AEC-044`, session
  `XNAT_E00043`, scan `2` ("Pelvis", 166 DICOM files, ~87 MB). Has a
  `DICOM` resource. Scan 2 of `XNAT_E00043` is the canonical "known to
  produce non-empty TotalSegmentator output" target.
- **Workflow id of the verified TS run:** `7951`. Container id `395`.
  ROI Collection assessor id `RoiCollection_sTTq83QI_2nA7pNW7j9C`.
- **Mirrored subset in this repo:**
  `datasets/xnat-tutorial/tcia_prostate_aec/archive.zip` — same
  scan, packaged for the `xnat_tutorial_plugin` loader.

For container debugging on demo.pro:

```bash
export XNAT_HOST=https://demo.pro.xnatworks.io
export COOKIE_JAR=/tmp/xnat_demo_pro_cookies.txt
# log in (will prompt for password)
curl -sS -u "jamesAdmin:<password>" -c "$COOKIE_JAR" "${XNAT_HOST}/data/JSESSION"
# example launch
curl -sS -b "$COOKIE_JAR" -X POST -H 'Content-Type: application/json' \
  "${XNAT_HOST}/xapi/projects/Prostate-AEC/wrappers/35/root/scan/launch" \
  -d '{"scan":"/experiments/XNAT_E00043/scans/2"}'
```

The launch path that works is
`/xapi/projects/{project}/wrappers/{wrapper-id}/root/{root-element}/launch`.
Other forms return HTTP 200 with a fake `workflow-id: "To be assigned"`
and don't actually launch.

## 6. Definition of Done Reminder

Before claiming any feature complete:

- [ ] Tested on real deployment (not just `shasum -c`).
- [ ] Screenshot or video captured as evidence (still pending — see §3.6).
- [ ] Backend API returns real data (not mocked).
- [ ] User can complete the full workflow end-to-end.

Specifically for this PR's outstanding items:

- [ ] PR #14 merged.
- [ ] `xnat_workbench` install verified on the public host before
      claiming `intro/09` and TS NIfTI variant work for end users.
- [ ] Plugin verdicts resolved and stale sources.yml entries pruned.

## 7. Instructions for New Session

```
Read HANDOFF.md and continue from where we left off.
First step: run `git status -sb` and `git log --oneline origin/main..HEAD`
to see the uncommitted README URL fix and the open PR #14 state.
```
