# Styling a deck

All of a deck's look comes from a few CSS custom properties set on `.cw-deck`. Override them for every deck, for one
deck, or for a group of decks, in your own stylesheet loaded after `catwalk.css`.

## The custom properties

| Property | Light (default) | Dark | Used for |
| --- | --- | --- | --- |
| `--cw-bg` | `#FBF8F2` | `#0B0F17` | The slide background |
| `--cw-bg-alt` | `#F2ECE0` | `#121826` | `data-cw-tone="alt"` slides, the transcript, highlighted contents |
| `--cw-ink` | `#1B1B1F` | `#EFE7D6` | Text |
| `--cw-muted` | `#5C5A55` | `#A9A39A` | Secondary text: sub lines, sources, captions |
| `--cw-accent` | `#8A6A1F` | `#C9A04A` | Kickers, buttons, markers, the progress already seen |
| `--cw-accent-2` | `#9A2F42` | `#E0788A` | The current slide in the progress bar, a wrong answer |
| `--cw-good` | `#2E7A4A` | `#6CC08A` | A right answer |
| `--cw-bar` | `#EFE8DA` | `#151B28` | The top and bottom bars |
| `--cw-font-display` | `"Libre Caslon Text", Georgia, serif` | | Headings and big lines |
| `--cw-font-body` | `"Libre Franklin", system-ui, sans-serif` | | Everything else |
| `--cw-radius` | `12px` | | Corners of the deck, buttons and questions |

The default fonts are used only if your page loads them. Otherwise the deck uses Georgia and the system's own sans-serif
font. To use your page's fonts:

```css
.cw-deck {
  --cw-font-display: inherit;
  --cw-font-body: inherit;
}
```

## The dark theme

Add `data-cw-theme="dark"` to a deck for the built-in dark colours, then override any of them as you like:

```html
<div class="cw-deck" id="night" data-cw-theme="dark" data-cw-title="Evening">...</div>
```

```css
#night {
  --cw-accent: #7FC8A9;
  --cw-accent-2: #F28C6B;
  --cw-radius: 4px;
}
```

## A theme of your own

Set the properties under your own attribute or class:

```css
.cw-deck.brand {
  --cw-bg: #FFFFFF;
  --cw-bg-alt: #EEF3FB;
  --cw-ink: #102A43;
  --cw-muted: #486581;
  --cw-accent: #1F6FEB;
  --cw-accent-2: #D9480F;
  --cw-good: #2B8A3E;
  --cw-bar: #E6EEF8;
}
```

Keep enough contrast between `--cw-ink` and `--cw-bg`, and between `--cw-bg` and `--cw-accent` (the button text), for
the text to be easy to read.

## Sizes and shape

A deck fills the width of its container. On a wide container its slides keep a 16:9 shape. When the deck is narrower
than 34rem, as on a phone, each slide grows to fit its content, between 60% and 85% of the screen's height. Text sizes
follow the deck's width (CSS container query units), so a deck in a narrow column scales down with it.

To limit a deck's width, size its container:

```css
.deck-column { max-width: 48rem; margin: 0 auto; }
```

The deck's margin, shadow and rounded corners are on `.cw-deck` and can be changed there.

## Motion

Slides fade and rise slightly as they change, and steps fade in. When the reader's system asks for reduced motion,
CatWalk uses none.

## Print and without JavaScript

Printed, a deck shows every slide one after another, with every step showing and without the player's bars.
Without JavaScript the deck is the same readable page: every slide, every step, each question's explanation behind
"Show the answer", and each slide's notes under it.

## Classes the player adds

These are on the elements the player adds, if you want to restyle them: `.cw-ready` (a started deck), `.cw-bar-top`,
`.cw-bar-bottom`, `.cw-viewport` (the slide area), `.cw-current` (the slide on show), `.cw-step-hidden`, `.cw-btn`,
`.cw-btn-primary` (Next), `.cw-progress`, `.cw-counter`, `.cw-contents`, `.cw-transcript`, `.cw-choice`, `.cw-right`,
`.cw-wrong` and `.cw-expanded` (the whole-window view).
