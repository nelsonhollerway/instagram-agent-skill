---
name: ig-carousel
description: >-
  Build an Instagram carousel - the cover that earns the swipe, slide-by-slide
  copy, and the 1080x1350 files to upload. Use when the user says "carousel",
  "slides", "swipe post", "turn this into a carousel", or has a list-shaped or
  step-shaped idea that would die as a single image.
---

# ig-carousel

Carousels are the highest-dwell format on the grid, because a swipe is an
interaction and a scroll is not. They also get a second chance: Instagram can
show a carousel again starting from a later slide to someone who did not engage
the first time, so slide two has to stand on its own as well.

The format rewards one idea broken into steps. It punishes a caption cut into
pieces.

## Voice

If the user names a page, brand or account for this request, look first for
`~/.claude/instagram/voice-<page>.md`, where `<page>` is that name lowercased
with spaces and punctuation turned to dashes, and use that instead of the
default. Otherwise read `~/.claude/instagram/voice.md` if it exists. If
neither file exists, ask which page this is for (when more than one
`voice-*.md` file exists in `~/.claude/instagram/`) and write in the voice the
user describes.

## When to use it instead of a Reel

Use a carousel when the idea has **sequence and needs to be re-read**: steps, a
framework with parts, a before and after, a list worth screenshotting. Use a
Reel when the idea has motion, a face, or a payoff that has to be seen
happening.

If the idea is one claim, it is neither. Hand it to `/ig-reel` and say so.

## Structure

6 to 10 slides. The cap is 20 and 20 is almost always a book nobody finishes.
Under 5 and the swipe never starts.

```
1         COVER     the hook. 6 words or fewer, at a size that is legible in
                    the grid at thumbnail. One line of promise under it.
2         THE STAKE why this matters, in one sentence. This slide is also a
                    second cover, so it cannot be setup.
3 to N    ONE IDEA PER SLIDE. A headline of 3 to 7 words, at most 25 words
                    under it. If a slide needs a paragraph, it is two slides.
N+1       RECAP     the whole thing as a list. This is the screenshot slide.
LAST      CTA       one action. Save, comment a keyword, or follow. One.
```

## Slide copy rules

- **The cover is 80% of the result.** Six words. Big. Nothing on the deck saves
  a cover nobody swipes.
- **Design for the grid crop.** The profile grid crops to a portrait rectangle
  that is taller than it is wide, and the exact ratio has moved more than once.
  Build at 1080x1350 and keep the cover text well inside the middle, clear of
  the outer 120 pixels on every side, and the crop stops mattering.
- **Number the slides** (3/8). Completion goes up when people can see the end.
- **No slide is a paragraph.** If it cannot be said in 25 words, split it.
- **The recap slide is the one people screenshot and send.** Sends are the
  strongest signal you can earn. Make it standalone and readable with no
  context.
- **The handle on every slide**, small, bottom corner. Screenshots travel
  without you.
- **Alt text on the cover at minimum.** It is read by screen readers and by
  Instagram.

## Building the files

Instagram wants 1080x1350 (4:5), JPEG or PNG, up to 20 items.

**Composite with `_tools/compose_carousel_slide.py`** (Pillow), not
HTML-to-image - a headless-Chrome or HTML-print pipeline needs a rendering
sandbox this project's environment does not reliably grant to local
file:// pages, backgrounds included. The compositor script takes a JSON
spec (one object per slide: `bg_path`, `out_path`, `eyebrow`, `headline`,
`sub`, `handle`, `pagenum`, `scrim` "top"/"bottom", optional `cta`) and
draws a dark gradient scrim plus the slide text directly onto the
generated background at full 1080x1350, including a permanent footer bar
so the handle and page number stay legible over any background.

```bash
python3 "_tools/compose_carousel_slide.py" slide_spec.json
```

A single accent colour, type no smaller than 32px (the script defaults are
already tuned to that), because this is read on a phone at a third of its
real size. If the project has a brand skill or a design system, use it and
do not invent a palette.

Generate the backgrounds first with `generate_image_batch`, download each
result, and compress large PNGs to JPEG (`sips -s format jpeg -s
formatOptions 55`) before compositing - keeps output files small without a
visible quality loss. Save everything under
`Drafts/<page>/Carousel/<treatment name>/`: raw generations in
`backgrounds/`, the finished slides in `final/`.

## AI generation prompts for the slide images

Write one image-generation prompt per slide, for whatever AI image tool the
user feeds them into. This is the background photography only - the slide
copy is added on top afterward as the HTML text layer, so **never put text
in the image prompt itself**, and say where the copy will sit so the
composition leaves it room (e.g. "clean negative space in the upper third
for a headline").

Per slide, one line: the concrete subject, the composition, the lighting/
mood consistent with the account's established look, 1080x1350 portrait
framing, and the text-safe zone. Name the actual thing in frame - "a
cinnamon-sugar-rimmed glass on a wood table" generates something; "a nice
drink" does not.

Same rule as the reel screenplay: this is production tooling for making
the image, not for disguising that it was AI-made.

## Output

The slide-by-slide copy first, as a numbered list the user can read in ten
seconds and edit before anything is rendered. Then the AI generation prompts,
one per slide. Then the **caption**, which for a carousel is Job B in
`/ig-caption`: the caption is doing work here, because the cover has already
used its six words.

Run both through `/ig-human`. Build the files only after the user approves the
copy.

```
CAROUSEL  ·  8 slides

1  COVER   THE $18,000 CLAUSE
           One line I now put in every contract.
2  STAKE   I approved the work. They asked for the money back nine days later.
3          WHAT IT SAYS
           Payment on delivery, not on approval.
...
7  RECAP   All four lines, in order.
8  CTA     Comment CONTRACT and I will send the full clause.

Caption: Job B, hook in line 1, one ask, 3 tags.
```

Nothing is uploaded. The user posts it.
