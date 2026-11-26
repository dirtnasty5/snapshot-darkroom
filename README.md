# SnapShot Darkroom

A one-page web app that turns SnapShot screenshots into polaroid prints.

Drop in screenshots → the page finds SnapShot's crop marks → cuts out the photo →
builds a fresh polaroid card around it → players add a caption, text, marker doodles,
stickers or tape → download as PNG or JPEG.

Everything runs in the player's browser. Nothing is uploaded anywhere.

**Live page:** `https://dirtnasty5.github.io/snapshot-darkroom/`

## Files

| File | What it is |
| --- | --- |
| `index.html` | The whole darkroom: page, styles and code in one file. |
| `README.md` | This file. |

## Crop mark spec (what the addon must draw)

The darkroom only finds marks that match this. `CropMarks.lua` in the addon draws them.

- **Four corner brackets**, one per photo corner, drawn just **outside** the photo.
  Each bracket's inner corner touches the photo's corner exactly; its two arms run
  along the photo's edges.
- **Colour:** pure magenta `#FF00FF`, fully opaque, with no filter, grain or vignette over it.
- **Outline:** near-black `#141414` on every side of each arm, `O` px wide.
- **Size**, in physical screen pixels (`H` = screen height):
  - arm length `L = max(24, 3% of H)`
  - arm thickness `T = max(6, 0.6% of H)`
  - outline `O = max(3, T / 2)`
- The brackets must be fully on screen, so frameless layouts need at least `T + O`
  pixels between the photo and the screen edge.
- Show them only for the instant of the screenshot.

The same four brackets work for every layout (square, portrait and landscape,
framed or frameless) because the darkroom reads the shape from the rectangle itself.

Screenshots without marks still work: the player clicks **Adjust crop** and drags the box.

## Tested

258 synthetic screenshots: 1280×720, 1366×768, 1920×1080, 2560×1440, 3440×1440
(ultrawide) and 5120×2880, all three shapes, framed and frameless sizes, JPEG quality
from 92% down to 40%, with busy backgrounds and pink decoy shapes. Every crop was
within 3 px; detection takes about 0.1 s per screenshot.
