# Budget Tracker — Claude Notes

## Project overview

A single-file static web app (`index.html`) for tracking personal income and expenses. No build step, no dependencies, no backend — everything runs in the browser.

## Architecture

- **Single file:** all HTML, CSS, and JS live in `index.html`
- **Persistence:** entries stored in `localStorage` under the key `budget-entries` as a JSON array
- **No frameworks:** vanilla JS only

### Data shape

Each entry stored in `localStorage`:

```js
{
  id:       1700000000000,   // Date.now() timestamp, used as unique key
  type:     'income' | 'expense',
  desc:     'Freelance payment',
  amount:   500.00,          // always positive; sign is derived from type
  category: 'Freelance',
  date:     'Jan 1, 2025'   // display-only string, not a sortable ISO date
}
```

### Category lists

Defined in the `CATEGORIES` constant — two arrays keyed by type. The select dropdown repopulates whenever the user switches type. To add or rename a category, update `CATEGORIES` and `ICONS` together.

## Common tasks

### Add a new category
1. Append the label to the relevant array in `CATEGORIES`
2. Add a matching emoji in `ICONS`

### Change the currency symbol or formatting
Edit the `fmt()` helper — it wraps `Intl.NumberFormat` via `toLocaleString`.

### Add a new field to entries (e.g. notes, date picker)
1. Add an input in the form HTML
2. Read it inside `addEntry()` and include it in the object pushed to `entries`
3. Render it inside the `.tx-detail` span in `render()`

### Persist to a backend instead of localStorage
Replace the `save()` function and the initial `entries` load with fetch calls. The rest of the code is unaffected.

## Style conventions

- CSS custom properties (variables) are defined on `:root` — change colours there, not inline
- Use `var(--green)` / `var(--red)` for semantic income/expense colouring throughout
- Animations use `@keyframes` injected dynamically to avoid cluttering the main `<style>` block (see `shake`)

## No build / test tooling

This is a plain static file. Open it directly in a browser to test:

```
open index.html        # macOS
xdg-open index.html    # Linux
start index.html       # Windows
```

There is no linter, bundler, or test suite configured. Keep the file self-contained.
