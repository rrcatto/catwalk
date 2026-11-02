# The JavaScript interface

CatWalk is one script, `catwalk.js`, with no dependencies. It defines `window.Catwalk` and starts every deck on the page
by itself.

## Starting decks

When the page has loaded, CatWalk starts every `.cw-deck` that does not have `data-cw-manual`. Decks that
[htmx](https://htmx.org) swaps into the page later are started too.

To start decks added any other way, call `Catwalk.mountAll()` with the element that holds them:

```js
container.innerHTML = deckHtml;
Catwalk.mountAll(container);
```

To start one deck yourself, with options, give it `data-cw-manual` so CatWalk leaves it alone, then mount it:

```html
<div class="cw-deck" id="tour" data-cw-manual data-cw-title="Tour">...</div>
<script src="catwalk.js"></script>
<script>
  var tour = Catwalk.mount(document.getElementById('tour'), {
    startAt: 3,
    links: {signup: '/join', continue: '/next'}
  });
</script>
```

## `Catwalk.mount(deck, options)`

Starts the player on one `.cw-deck` element and returns its handle. It returns the existing handle if the deck is
already started, and `null` if the element is not a deck or has no slides.

| Option | What it does |
| --- | --- |
| `startAt` | The slide to open on, counting from 1. Defaults to 1. |
| `links` | Addresses for the [action buttons](markup.md#action-buttons), by action name. They add to, and win over, the deck's `data-cw-host-links`. |

## `Catwalk.mountAll(container)`

Starts every deck inside `container` (the whole document if left out) that is not started yet and does not have
`data-cw-manual`. Returns the handles of the decks it started or found started.

## `Catwalk.version`

The version of CatWalk, such as `"1.0.1"`.

## A deck's handle

| Method | What it does |
| --- | --- |
| `go(n)` | Shows slide `n`, counting from 1. |
| `next()` | Reveals the next step, or moves to the next slide. The same as the Next button. |
| `prev()` | Moves to the previous slide, with its steps shown. |
| `destroy()` | Removes the player: its bars, buttons and listeners go, and the deck is the readable page it was before. |

```js
var deck = Catwalk.mount(document.querySelector('#tour'));
document.querySelector('#restart').addEventListener('click', function () { deck.go(1); });
```

## Events

Each deck sends events as the reader uses it. They are `CustomEvent`s that bubble, so one listener on `document` hears
every deck. The details are in `event.detail`, and every event carries the deck's `data-cw-key` as `key`.

| Event | When | `detail` |
| --- | --- | --- |
| `cw:ready` | The player has started | `key`, `kind` (the deck's `data-cw-kind`), `total` (number of slides) |
| `cw:slide` | A slide is shown | `key`, `index` (from 0), `total`, `title` |
| `cw:check` | A quick question is answered | `key`, `index` (the slide, from 0), `chosen` (the option, from 0), `correct` (true or false) |
| `cw:action` | An action button is clicked | `key`, `action` (its `data-cw-action`), `href` (where it leads) |
| `cw:complete` | The last slide is reached, once | `key`, `seconds` since the player started |

```js
document.addEventListener('cw:complete', function (event) {
  console.log('Finished ' + event.detail.key + ' in ' + event.detail.seconds + ' seconds');
});

document.addEventListener('cw:check', function (event) {
  if (!event.detail.correct) { console.log('Missed the question on slide ' + (event.detail.index + 1)); }
});
```

CatWalk itself sends nothing over the network: if you want a record of what readers do, listen for these events and
send it yourself.

`cw:action` is sent while the click is handled, before the browser follows the link, so it suits recording which
button a reader chose.

## Keys, clicks and swipes

The keys act only while focus is inside the deck, so several decks on one page never answer the same key, and the
page's own keys work as usual elsewhere.

| Key | What it does |
| --- | --- |
| Right arrow, Page Down, Space | Next step or slide |
| Left arrow, Page Up | Previous slide |
| Home | First slide |
| End | Last slide, with its steps shown |
| F | Full screen, or out of it |
| M | Narration on or off (when the deck has narration) |
| Escape | Closes Contents, or leaves the whole-window view |

A click or tap on a slide, away from its links, buttons and questions, moves on like Next. A swipe left moves on and a
swipe right goes back. Keys pressed in a form field inside a slide are left to the field.

## Full screen

The Full screen button (or F) shows the deck on the whole screen. Where the browser has no full screen for page
elements, as on some phones, the deck fills the browser window instead, and Close or Escape returns it.
