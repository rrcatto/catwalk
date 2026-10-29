# CatWalk

A small presentation engine for the web. You write slides as plain HTML, and CatWalk plays them one at a time, on a
phone or a large screen, with points that build, quick questions, chapters, a contents list, full screen, narration and
a transcript.

- **Two files, no dependencies.** `catwalk.js` and `catwalk.css`, about 9 KB together when compressed. No build step,
  no framework, no network requests of its own.
- **Plain HTML in, readable page out.** Without JavaScript, and in print, a deck is one page with every slide in order.
  The player only adds its controls around your slides and never changes them.
- **Phone first.** A deck fits its container: 16:9 on a wide screen, growing to fit its content on a phone. Text scales
  with the deck's width.
- **Accessible.** Keys, clicks and swipes, buttons large enough to tap, announcements for screen readers, hidden slides
  kept out of reach, and no motion for readers who ask for less.
- **Several decks on one page.** Each answers only its own keys.
- **Scriptable.** Start, drive and remove decks from JavaScript, and listen for what readers do.

## Quick start

Download `catwalk.css` and `catwalk.js` into your site, then write a deck:

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <link rel="stylesheet" href="catwalk.css">
</head>
<body>
  <div class="cw-deck" data-cw-title="Hello, CatWalk">
    <section class="cw-slide" data-cw-title="Welcome">
      <p class="cw-kicker">A first deck</p>
      <h2>Slides are plain HTML</h2>
      <p class="cw-sub">Each section is one slide.</p>
    </section>

    <section class="cw-slide" data-cw-title="Points">
      <h2>Next reveals one point at a time</h2>
      <ul>
        <li class="cw-step">First point</li>
        <li class="cw-step">Second point</li>
      </ul>
    </section>

    <section class="cw-slide" data-cw-title="A question">
      <h2>Which comes first?</h2>
      <ol class="cw-options" data-cw-answer="0">
        <li>The first point</li>
        <li>The second point</li>
      </ol>
      <details class="cw-explain">
        <summary>Show the answer</summary>
        <p>The first point: it is first in the list.</p>
      </details>
    </section>
  </div>

  <script src="catwalk.js"></script>
</body>
</html>
```

That is all. CatWalk starts every `.cw-deck` on the page when it loads.

## How it works

- **A deck** is a `div.cw-deck`. Its `data-cw-title` names it.
- **A slide** is a `section.cw-slide` directly inside the deck. Its `data-cw-title` is its name in Contents.
- **Steps:** any element with the class `cw-step` stays hidden until Next reveals it.
- **Chapters:** `data-cw-chapter` on a slide starts a chapter, shown in the top bar, in Contents and in the progress bar.
- **Quick questions:** an `ol.cw-options` with `data-cw-answer` (the right option, counting from 0) becomes buttons,
  and the `details.cw-explain` after it opens once the reader chooses.
- **Buttons:** an `a.cw-action` link is a button. Its `data-cw-action` names its address, given by the deck's
  `data-cw-host-links`, or `goto` jumps to another slide.
- **Narration:** `data-cw-audio` on a slide is the address of its recording. A Narration button appears, and plays each
  slide's recording as it is shown.
- **Transcript:** an `aside.cw-notes` in a slide is its transcript, shown under the slides by the Transcript button.

The stylesheet also styles twelve layouts: title, statement, points, compare, picture, number, timeline, steps, case,
quick question, up next and call to action. Each is a pattern of a few classes, described in
[docs/markup.md](docs/markup.md).

## Using it

| | |
| --- | --- |
| Next | Right arrow, Page Down, Space, a click or tap on the slide, a swipe left |
| Previous | Left arrow, Page Up, a swipe right |
| First and last slide | Home and End |
| Full screen | F, or the Full screen button |
| Narration | M, or the Narration button |
| Close Contents or the whole-window view | Escape |

The keys work while focus is in the deck: click it, or move to it with Tab.

## From JavaScript

```js
// Start a deck that has data-cw-manual, opening on slide 2, with addresses for its buttons.
var tour = Catwalk.mount(document.getElementById('tour'), {
  startAt: 2,
  links: {signup: '/join'}
});

tour.next();      // the next step or slide
tour.go(1);       // slide 1
tour.destroy();   // back to the plain page

// Decks added to the page later.
Catwalk.mountAll(container);

// What readers do.
document.addEventListener('cw:complete', function (event) {
  console.log(event.detail.key + ' finished in ' + event.detail.seconds + ' seconds');
});
```

The events are `cw:ready`, `cw:slide`, `cw:check`, `cw:action` and `cw:complete`. See
[docs/javascript.md](docs/javascript.md).

## Styling

Colours, fonts and corners come from CSS custom properties on `.cw-deck`, such as `--cw-bg`, `--cw-ink` and
`--cw-accent`. `data-cw-theme="dark"` gives the built-in dark colours.

```css
.cw-deck {
  --cw-accent: #1F6FEB;
  --cw-font-body: inherit;
}
```

See [docs/styling.md](docs/styling.md).

## Examples

Open [examples/index.html](examples/index.html):

- [minimal.html](examples/minimal.html): the smallest deck.
- [layouts.html](examples/layouts.html): one slide of each layout, with chapters, tones, a picture and transcripts.
- [scripting.html](examples/scripting.html): two decks on one page, a deck started by script, buttons that drive it,
  the events, and a restyled dark deck.

## Browser support

Current versions of Chrome, Edge, Firefox and Safari, on computers and phones. CatWalk uses CSS container queries,
`color-mix()` and the `inert` attribute, which these browsers have all had since 2023. Where a browser has no full screen
for page elements (Safari on iPhone), the deck fills the browser window instead.

## Documentation

- [Writing a deck](docs/markup.md): the deck, slides, steps, questions, buttons, narration and every layout.
- [The JavaScript interface](docs/javascript.md): starting decks, the handle, events, keys.
- [Styling a deck](docs/styling.md): custom properties, the dark theme, sizes, print.
- [Changelog](CHANGELOG.md)

## Licence

MIT. See [LICENSE](LICENSE).
