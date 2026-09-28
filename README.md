# Recipe with adjustable servings

A recipe page (petulla, Albanian fried dough) with an ingredient list, a method, and a servings control that scales every quantity. Built with plain HTML, CSS and JavaScript. No build step, no dependencies.

## Run it

Open `index.html` in a browser. Or serve the folder:

```bash
npx serve .
```

## What it does

- Change servings (1 to 24) with the minus and plus buttons or by typing a number.
- Quantities are written once, in `index.html`, for 4 servings (`data-qty` and `data-unit`). The script multiplies them and rounds sensibly: grams and milliliters to whole numbers, spoons to quarters, thirds and halves.
- Items with no fixed quantity (oil for frying, feta to serve) do not scale.

## Accessibility decisions

**Keyboard.** Tab order follows the visual order: skip link, servings control, ingredients, method. All controls are native `button` and `input` elements, so Enter, Space, and the arrow keys in the number field work without extra code.

**Focus.** Every interactive element gets a 3px outline with an offset on `:focus-visible`. The colour is separate from the button colours so it stays visible on both the light and dark themes.

**Quantity changes are announced.** The quantities are plain text, so a screen reader would say nothing when they change. A visually hidden `role="status"` (polite live region) receives a sentence such as "Quantities updated for 6 servings. 750 grams plain flour. ..." with units spelled out. The announcement is debounced by 500 ms, so ten quick clicks produce one announcement rather than a queue. It stays silent on page load.

**Limits without losing focus.** At 1 and 24 the buttons use `aria-disabled` instead of `disabled`. A disabled button that has focus drops it, which sends keyboard users back to the top of the page.

**320px.** No fixed widths wider than the viewport, flexible grid columns with `minmax(0, ...)`, `overflow-wrap: anywhere` for long words, and text-size adjustment left on. The page has no horizontal scroll at 320px.

**Other.** Skip link, `lang` set, heading hierarchy (h1, h2), dark theme through `prefers-color-scheme`, reduced motion respected, touch targets of at least 44px.

## Narrow screen: ingredients and method

Side by side works on a wide screen, but on a phone the two sections stacked end up far apart: a cook needs the ingredient list while reading step 4, and scrolling back and forth loses their place.

Below 720px only one section is shown at a time. A sticky two-button switch ("Ingredients" / "Method") stays at the top of the screen while scrolling, so the other section is always one tap away and the scroll position of the page stays short. The servings control sits above the switch, so quantities can be changed and then viewed without moving.

The switch uses two toggle buttons with `aria-pressed` and `aria-controls`. The hidden section uses the `hidden` attribute, so it is removed from the accessibility tree and the tab order, rather than just being moved off screen. From 720px up, the switch is hidden and both sections are shown next to each other.

Tradeoff: a reader cannot see both at once on a phone. I chose this over an accordion because an open accordion still pushes the second section far down the page.

## Manual checks I ran

- Keyboard only, from the skip link to the last step, without a mouse.
- 320px wide in the browser dev tools: no horizontal scroll.
- Screen reader : quantity announcement after pressing plus.