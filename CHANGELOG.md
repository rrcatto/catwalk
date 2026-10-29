# Changelog

All notable changes to CatWalk are listed here. Versions follow [Semantic Versioning](https://semver.org).

## 1.0.0 - 2026-10-29

The first version.

### Player

- Plays each `div.cw-deck` of `section.cw-slide` elements one slide at a time, starting every deck on the page by itself.
- Steps: elements with `cw-step` are revealed one press of Next at a time, and `data-cw-step` groups them.
- Quick questions: an `ol.cw-options` with `data-cw-answer` becomes buttons that mark the right answer and the reader's
  choice, then open the `details.cw-explain` explanation.
- Chapters (`data-cw-chapter`) in the top bar, in Contents and as gaps in the progress bar.
- A Contents list of every slide, by chapter.
- Full screen, falling back to the whole browser window where the browser has no full screen for page elements.
- Narration: a recording per slide (`data-cw-audio`), played when narration is turned on, with optional automatic
  advance (`data-cw-advance="auto"`).
- A transcript from each slide's `aside.cw-notes`.
- Action buttons (`a.cw-action`) whose addresses come from `data-cw-host-links` or the `links` option, and `goto`
  buttons that jump to another slide.
- Keys (arrows, Page Up and Down, Space, Home, End, F, M, Escape) that act only while focus is in the deck, clicks and
  taps that move on, and swipes.
- Screen reader support: the deck as a region of labelled slides, announcements of each slide, step and answer, and
  slides and steps out of sight kept out of reach.

### Interface

- `Catwalk.mount(deck, {startAt, links})`, `Catwalk.mountAll(container)` and `Catwalk.version`.
- A handle with `go(n)`, `next()`, `prev()` and `destroy()`.
- The events `cw:ready`, `cw:slide`, `cw:check`, `cw:action` and `cw:complete`.
- `data-cw-manual` to start a deck from script, and decks swapped in by htmx started automatically.

### Styling

- Twelve layouts: title, statement, points, compare, picture, number, timeline, steps, case, quick question, up next and
  call to action.
- Light and dark themes, set by CSS custom properties, and slide tones (`alt`, `accent`).
- Phone first: 16:9 on wide screens, slides that grow to fit on narrow ones, and text that scales with the deck.
- Reduced motion followed, and a print layout of every slide in order.
- Without JavaScript, a readable page of every slide.
