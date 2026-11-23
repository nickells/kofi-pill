# kofi-pill

A "Buy me a coffee" pill and Ko-fi tip panel for any page, in one dependency-free script.

- **It waits.** The pill rises into the bottom-right corner once a visitor has scrolled a few screens or clicked a few controls (links, buttons, form fields), not on page load.
- **One ask at a time.** If the page has its own Ko-fi link (say, in the nav), the pill stays away while that link is on screen.
- **Tips happen on the page.** The pill and your own links open Ko-fi's tip form in a dialog instead of sending visitors away. The form loads the first time someone opens it, so pages where nobody does make no request to Ko-fi, and none of Ko-fi's scripts run on your page.
- **Closing means closed.** The × hides the pill for 30 days (in `localStorage`).

## Use

Load it at the end of `<body>`, from jsDelivr at a fixed version:

```html
<script src="https://cdn.jsdelivr.net/gh/nickells/kofi-pill@v1.0.0/kofi-pill.js" defer data-kofi-id="YOUR_KOFI_ID"></script>
```

…or copy `kofi-pill.js` into your site and load it from there, so pages make no request to a CDN.

`YOUR_KOFI_ID` is the id in your Ko-fi page's URL: `ko-fi.com/A7S62B6EOC` → `A7S62B6EOC`.

### Your own link

Mark any link to your Ko-fi page with `data-kofi`. It opens the panel too, and the pill keeps out of its way. The attribute's value names the link in click events. Add `data-kofi-cup` to lead it with the mug icon:

```html
<a href="https://ko-fi.com/YOUR_KOFI_ID" data-kofi="nav" data-kofi-cup>Buy me a coffee</a>
```

## Options

Set them as data attributes on the script tag:

```html
<script src="kofi-pill.js" defer
  data-kofi-id="YOUR_KOFI_ID"
  data-ask="Having fun?"
  data-cta="Buy me a coffee"
  data-caption="Tips keep the servers on."></script>
```

…or, to share them across pages from one file, as a `window.kofiPill` object set before the script runs (attributes win over it):

```js
window.kofiPill = { kofiId: "YOUR_KOFI_ID", ask: "Having fun?", caption: "Tips keep the servers on." };
```

| Attribute | Object key | Default | What it does |
| --- | --- | --- | --- |
| `data-kofi-id` | `kofiId` | *(required)* | Your Ko-fi page id. |
| `data-ask` | `ask` | `Useful?` | The lead-in before the call to action. Empty for none. |
| `data-cta` | `cta` | `Buy me a coffee` | The pill's call to action, in bold. |
| `data-caption` | `caption` | *(none)* | A line above the tip form in the panel. |
| `data-after-screens` | `afterScreens` | `3` | Screens scrolled before the pill appears. |
| `data-after-clicks` | `afterClicks` | `5` | Clicks on controls before the pill appears. |
| `data-hide-days` | `hideDays` | `30` | Days the pill stays away after it's closed or clicked. |

## Styling

The defaults are near-black ink on a white page. Override any of these custom properties in your own CSS:

```css
:root {
  --kofi-pill-ink: #151515;      /* the pill's background, the panel's close button */
  --kofi-pill-on-ink: #fff;      /* the pill's text */
  --kofi-pill-close: #e2e0d8;    /* the pill's × */
  --kofi-pill-accent: currentColor; /* the underline on hover */
  --kofi-pill-muted: #5f5e58;    /* the panel's caption */
  --kofi-pill-hover: #f2f1ec;    /* the panel's close button on hover */
  --kofi-pill-font: inherit;
  --kofi-pill-radius: 6px;       /* the panel's corners */
  --kofi-pill-z: 60;             /* the pill's stacking order */
}
```

The tip form inside the panel is Ko-fi's own, so its look and wording come from your Ko-fi settings, not from here.

## Analytics

Every click on the pill or a `data-kofi` link fires a `kofi-pill:click` event on `document`, with `detail.from` set to the link's `data-kofi` value (`"pill"` for the pill):

```js
document.addEventListener("kofi-pill:click", (e) => posthog.capture("kofi_click", e.detail));
```

## License

MIT
