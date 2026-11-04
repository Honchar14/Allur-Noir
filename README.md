# Allure Noir

First screen of the Allure Noir perfume website: an animated preloader (marker-drawn cupid and signature, counter, arrow shot) that hands off to the main screen (rolling title, typewriter copy, looping product videos).

Static site, no build step: `index.html` + `media/`.

```bash
npx serve .
```

## Layouts

- **Desktop** (viewport ≥ 960×540): the main screen is a 1920×1080 artboard scaled to fit; the background and grain run edge to edge, so other aspect ratios show no bars or seams.
- **Mobile / narrow** (anything smaller, including tablets held upright and phones in landscape): the main screen becomes a scrollable single-column layout (two text columns from 600 px wide). The paragraphs reveal when scrolled into view.
- **Preloader**: has its own artboard that always fills the screen. It's landscape on wide screens and portrait (1080 units wide) on upright screens. The counter sits in the bottom-left corner, the signature at the top, the cupid in the centre.

## Media

| file | used for |
|---|---|
| `cupid-aim.webm/.mp4` | preloader cupid turn (white on black, composited opaque) |
| `loop.webm/.mp4` | big product loop, used on screens where the box is wider than 1100 device px |
| `loop-m.webm/.mp4` | 1024×768 cut of the same loop for phones, tablets and 1× desktops |
| `macro.webm/.mp4` | small square loop |

The main videos start downloading only after the preloader clip (`preload="none"` → raised by script). They pause when scrolled off-screen or when the tab is hidden.

## How the intro runs

- One master clock. Web Animations play natively, so transforms and opacity run on the compositor. JS drives only the counter reels, the title motion blur, the cupid clip sync and the typewriter.
- If the cupid clip isn't ready at 2.35 s, the intro holds on the drawn cupid (counter paused) until it can play. If autoplay is blocked (e.g. iPhone Low Power Mode) or the clip fails, the vector cupid stays and the intro finishes without the clip.
- A background tab pauses the intro. It resumes when the tab is shown.
- `prefers-reduced-motion` stops the endless loops (grain jitter, watermark wings).

## QA hooks

- `index.html#noplay`: loads without playing. Then call `__seek(ms)` in the console to freeze the whole intro at any time.
- `__t()`: the current intro clock in ms.
