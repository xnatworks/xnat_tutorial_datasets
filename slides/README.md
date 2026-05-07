# Slides

Auto-generated PowerPoint deck covering the lesson catalog. Designed
for copy-paste into a templated deck — formatting is intentionally
minimal so it inherits your master slides cleanly.

## Files

- `xnat-tutorials.pptx` — 34-slide deck. One slide per lesson plus
  title / overview / section dividers / closing.
- `build_tutorial_slides.py` — generator script. Reads the lesson
  markdown files under `../tutorials/` and rebuilds the deck.

## Regenerate

```bash
python3 slides/build_tutorial_slides.py
```

Requires `python-pptx` (`pip install python-pptx`).

## What goes on each lesson slide

- Title (lesson H1).
- Recommended dataset.
- One-line goal.
- First five walkthrough steps.
- Verify check.
- Source markdown path in speaker notes.

## Editing

Edit the lesson markdown under `tutorials/`, then re-run the
generator. Do not hand-edit `xnat-tutorials.pptx` — it gets
overwritten.
