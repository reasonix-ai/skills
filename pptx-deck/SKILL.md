---
name: pptx-deck
description: Plan and build a PowerPoint (.pptx) deck from the user's material - one claim per slide, plain wording, a real layout per slide, then generate the file with the bundled script. Use when asked for slides, a presentation or a .pptx.
---

# pptx-deck

Turn the user's material into a deck that looks made by a person who knows the
subject, not by a template. Work in this order and do not skip the first two
steps; a deck built straight from a blank spec is where the generic look comes from.

## 1. Pin down the deck

Ask only what you cannot infer, in one message: who will see it, what they
should do or decide afterwards, how long it runs, and whether a template or
brand colour is given. If the material is a file, read it first. Write the
answer to "what should the audience do after the last slide" as one sentence;
every slide must serve it.

## 2. Outline as claims

Write the outline as one full sentence per slide stating that slide's point
(the headline), not a topic label. Cut every slide whose sentence does not
support the goal. Typical length: one slide per minute of speaking, plus a
title and a closing slide. Show the outline and wait for a go-ahead or edits
before building.

## 3. Choose a layout per slide

Pick from the layouts the script supports and vary them: `title`, `section`,
`points` (headline plus up to 5 short bullets), `two-column`, `image` (one
picture, optional caption), `table`, `quote`, `numbers` (up to 4 big figures
with labels). Never use `points` for more than two slides in a row. Follow
`references/style.md` for wording and visual rules; read it before writing any
slide text.

## 4. Write the spec

Write a JSON file with the slides (format below). Put the longer explanation in
`notes`, not on the slide. Use only images the user supplied or that you made
locally; never fetch images or fonts from the network.

```json
{
  "title": "Deck title",
  "theme": {"font": "Calibri", "accent": "1F4E79", "ink": "1A1A1A", "paper": "FFFFFF"},
  "slides": [
    {"layout": "title", "title": "...", "subtitle": "..."},
    {"layout": "points", "title": "A full-sentence claim", "bullets": ["...", "..."], "notes": "..."},
    {"layout": "numbers", "title": "...", "items": [{"value": "42%", "label": "..."}]},
    {"layout": "two-column", "title": "...", "left": {"heading": "...", "bullets": []}, "right": {"heading": "...", "bullets": []}},
    {"layout": "image", "title": "...", "image": "chart.png", "caption": "..."},
    {"layout": "table", "title": "...", "header": ["..."], "rows": [["..."]]},
    {"layout": "quote", "quote": "...", "by": "..."},
    {"layout": "section", "title": "..."}
  ]
}
```

## 5. Build and check

The script needs the `python-pptx` package. If `python3 -c "import pptx"` fails,
tell the user the one command to install it (`python3 -m pip install python-pptx`,
preferably in a virtual environment) and let them run it; do not install packages
yourself.

```
python3 scripts/build_deck.py spec.json out.pptx
```

The script prints a warning for each slide that breaks a size rule (too many
bullets, long bullets, long headline, missing image). Fix the warnings by
cutting words, not by shrinking text. If LibreOffice is installed
(`soffice --headless --convert-to pdf out.pptx`), convert and look at the pages;
otherwise say the deck was not visually checked.

## 6. Hand over

Give the path, the slide count, any warning that remains and why it stays, and
what you could not check. Offer to adjust the outline rather than the visuals first.

## Rules

- Only read the files the user pointed to and write the spec and the output next
  to them; run nothing else from those files.
- Do not invent figures, quotes or sources. A number on a slide comes from the
  user's material or is marked as an assumption in the notes.
- The scripts are plain text in `scripts/`; read `build_deck.py` before running it
  if the user asks what it does.
