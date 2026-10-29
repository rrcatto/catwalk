# Writing a deck

A CatWalk deck is ordinary HTML: a `div.cw-deck` holding one `section.cw-slide` per slide. CatWalk reads it, adds its
controls around it and never changes what you wrote. Without JavaScript, and in print, the same HTML reads as one page
with every slide in order.

```html
<div class="cw-deck" data-cw-title="My first deck">
  <section class="cw-slide" data-cw-title="Welcome">
    <h2>Hello</h2>
  </section>
  <section class="cw-slide" data-cw-title="Next">
    <h2>A second slide</h2>
  </section>
</div>
```

The slides must be direct children of the deck. A deck with no slides is left as it is.

## The deck

| Attribute | What it does |
| --- | --- |
| `class="cw-deck"` | Required. Marks the deck. |
| `data-cw-title` | The deck's name, shown in its top bar and given to screen readers. Defaults to "Presentation". |
| `data-cw-theme="dark"` | Uses the built-in dark colours. Leave it out for the light ones. See [styling](styling.md). |
| `data-cw-key` | Any identifier you choose. It comes back in every [event](javascript.md#events), so a page with several decks can tell them apart. |
| `data-cw-kind` | Any label you choose, such as `intro` or `review`. It comes back in the `cw:ready` event. |
| `data-cw-host-links` | A JSON object of addresses for the action buttons, by action name, such as `{"continue": "/next-page", "signup": "/join"}`. See [action buttons](#action-buttons). |
| `data-cw-advance="auto"` | With narration on, moves on when a slide's recording ends. See [narration](#narration). |
| `data-cw-manual` | CatWalk does not start this deck by itself: you start it with `Catwalk.mount()`. |
| `tabindex` | Added as `0` if missing, so the deck can take focus and answer keys. |
| `lang` | Set it when the deck's language differs from the page's. |

## A slide

| Attribute | What it does |
| --- | --- |
| `class="cw-slide"` | Required, on a `section`. |
| `data-cw-title` | The slide's short name, used in Contents and announced to screen readers. |
| `data-cw-chapter` | Starts a chapter at this slide. Its name shows in the top bar until the next chapter, as a heading in Contents, and as a gap in the progress bar. |
| `data-cw-tone` | `alt` for the second background colour, `accent` for the accent colour. |
| `data-cw-layout` | The slide's layout, such as `points` or `picture`. A hook for styling: see [layouts](#layouts). |
| `data-cw-audio` | The address of the slide's narration recording. See [narration](#narration). |
| `id` | Lets an action button jump to this slide. See [action buttons](#action-buttons). |

## Points that build

Give any element inside a slide the class `cw-step` and it stays hidden until Next is pressed. Each press shows the next
step, in the order they appear. After the last step, Next moves to the next slide. Going back to a slide shows all of
its steps.

```html
<ul>
  <li class="cw-step">Shown on the first press of Next</li>
  <li class="cw-step">Shown on the second</li>
</ul>
```

To show several elements at once, number them with `data-cw-step`. Elements with the same number appear together, in
number order:

```html
<li class="cw-step" data-cw-step="1">These two</li>
<li class="cw-step" data-cw-step="1">appear together</li>
<li class="cw-step" data-cw-step="2">and this one after them</li>
```

## A quick question

An `ol.cw-options` with `data-cw-answer` becomes a question the reader answers on the slide. `data-cw-answer` is the
right option's position, counting from 0. Each list item becomes a button. Once one is chosen, the right answer and the
reader's choice are marked, and the explanation in the `details.cw-explain` that follows opens. Nothing is recorded or
sent anywhere, though the page may listen for the [`cw:check` event](javascript.md#events).

```html
<section class="cw-slide" data-cw-layout="check" data-cw-title="A quick question">
  <h2>Which planet is closest to the sun?</h2>
  <ol class="cw-options" data-cw-answer="2">
    <li>Venus</li>
    <li>Earth</li>
    <li>Mercury</li>
  </ol>
  <details class="cw-explain">
    <summary>Show the answer</summary>
    <p>Mercury, at about 58 million kilometres.</p>
  </details>
</section>
```

Without JavaScript the options stay a numbered list and the explanation opens with its summary. The slide's `h2` labels
the question for screen readers.

## Action buttons

A link with the class `cw-action` is a button on the slide. Its `data-cw-action` names what it does:

- **Any name you choose**, such as `continue` or `signup`. If the deck's `data-cw-host-links` (or the `links` option of
  `Catwalk.mount()`) gives an address for that name, the link goes there. Otherwise it keeps its own `href`.
- **`goto`** jumps to another slide in the same deck, named by its `id` in `data-cw-target`. Give it an `href` to the
  same `id` so it also works without JavaScript.

```html
<div class="cw-deck" data-cw-title="Plans" data-cw-host-links='{"signup": "/join"}'>
  ...
  <section class="cw-slide" data-cw-title="Join us">
    <h2>Ready to start?</h2>
    <p class="cw-actions">
      <a class="cw-action" data-cw-action="signup" href="#">Sign up</a>
      <a class="cw-action" data-cw-action="goto" data-cw-target="prices" href="#prices">See the prices again</a>
    </p>
  </section>
</div>
```

Every click on an action button sends a [`cw:action` event](javascript.md#events).

## Narration

A slide's `data-cw-audio` is the address of a recording of its narration, in any format the browser plays (MP3, Ogg,
WebM, M4A, WAV). When any slide has one, the player shows a Narration button. With narration on, each slide's
recording plays when the slide is shown and stops when the reader moves on. The recording never plays until the reader
turns narration on, because browsers refuse sound that nobody asked for.

Add `data-cw-advance="auto"` to the deck to move on by itself when a recording ends. This suits slides without steps:
on a slide with steps, the end of its recording reveals one step, and the reader presses Next for the rest.

## Transcript

An `aside.cw-notes` inside a slide is its transcript, usually the words of its narration. In the player it is hidden,
and a Transcript button shows the current slide's notes under the slides. Without JavaScript the notes show under their
slide.

```html
<section class="cw-slide" data-cw-title="Welcome" data-cw-audio="audio/welcome.mp3">
  <h2>Welcome</h2>
  <aside class="cw-notes">Welcome. In the next two minutes you will see how it works.</aside>
</section>
```

## Layouts

A slide can hold any HTML. CatWalk's stylesheet also styles twelve layouts, each a pattern of a few classes. The
`data-cw-layout` attribute is only a styling hook: `statement` sets a smaller big line than `number`, and `picture`
lets its picture fill the slide. [examples/layouts.html](../examples/layouts.html) has one slide of each.

| Layout | For | Its parts |
| --- | --- | --- |
| `title` | Opening the deck or a chapter | `p.cw-kicker` (small line above), `h2`, `p.cw-sub` (line under) |
| `statement` | One big idea or a quotation | `p.cw-big`, `p.cw-source` |
| `points` | A short list that builds | `h2`, a `ul` or `ol` of `li.cw-step` |
| `compare` | Two things side by side | `h2`, `div.cw-cols` of two `div.cw-col`, each an `h3` with a list or a paragraph |
| `picture` | A diagram, photo or chart | `h2`, `figure.cw-figure` holding an `img` or a `div.cw-svg` with an inline `svg`, and a `figcaption` |
| `number` | One striking figure | `p.cw-big` (the figure), `p.cw-label` (what it means), `p.cw-source` |
| `timeline` | Events in order | `h2`, `ol.cw-timeline` of `li.cw-step`, each starting with `b.cw-when` |
| `steps` | A process to follow | `h2`, `ol.cw-steps` of `li.cw-step` (numbered circles) |
| `case` | A scenario, its question, then what happened | `h2`, `div.cw-facts`, `p.cw-question`, `div.cw-outcome.cw-step` (revealed by Next) |
| `check` | A quick question | `h2` (the question), `ol.cw-options`, `details.cw-explain` |
| `next` | Handing over to what follows | `h2`, `p`, `a.cw-action` |
| `cta` | Asking the reader to act | `h2`, `p`, `p.cw-actions` of `a.cw-action` links |

A few examples:

```html
<section class="cw-slide" data-cw-layout="statement" data-cw-title="The big idea" data-cw-tone="alt">
  <p class="cw-big">Water is never used up. It only moves and changes form.</p>
  <p class="cw-source">The idea behind the water cycle</p>
</section>

<section class="cw-slide" data-cw-layout="compare" data-cw-title="Two kinds">
  <h2>Tea or coffee?</h2>
  <div class="cw-cols">
    <div class="cw-col"><h3>Tea</h3><ul><li>Steeped leaves</li><li>Less caffeine</li></ul></div>
    <div class="cw-col"><h3>Coffee</h3><p>Roasted, ground beans, brewed with hot water.</p></div>
  </div>
</section>

<section class="cw-slide" data-cw-layout="picture" data-cw-title="The map">
  <h2>Where we are</h2>
  <figure class="cw-figure">
    <img src="map.png" alt="A map of the town centre with the library marked">
    <figcaption>The library is two streets from the station.</figcaption>
  </figure>
</section>

<section class="cw-slide" data-cw-layout="number" data-cw-title="A figure">
  <p class="cw-big">8 minutes</p>
  <p class="cw-label">for sunlight to reach the Earth</p>
</section>
```

For an inline SVG picture, wrap it in `div.cw-svg` and label it, since the drawing itself is not read aloud:

```html
<div class="cw-svg" role="img" aria-label="A bar chart of sales rising each month">
  <svg viewBox="0 0 400 200" xmlns="http://www.w3.org/2000/svg">...</svg>
</div>
```

Give an SVG a `viewBox` so it scales to the slide.

## Keeping slides short

A slide is read on a phone as well as a large screen. CatWalk scales its text with the deck's width, and a slide fuller
than its box scrolls, but slides read best with a heading of up to about ten words, up to five points of up to about
twelve words each, and a picture that is clear at a small size.
