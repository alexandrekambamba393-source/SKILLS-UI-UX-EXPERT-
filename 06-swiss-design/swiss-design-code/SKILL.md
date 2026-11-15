---
name: swiss-design-code
description: Production-ready HTML and CSS implementation of a Swiss-style layout with a 12-column CSS grid, a modular type scale, flush-left text blocks and a restrained accent system. Use when building a precise, grid-driven page in plain HTML and CSS.
---

# Swiss Design: HTML and CSS Implementation

```html
<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Swiss Layout</title>
<style>
  :root {
    --paper: #ffffff;
    --ink: #0a0a0a;
    --grey: #6e6e6e;
    --rule: #d8d8d8;
    --accent: #e10600;
    --gutter: 24px;
    --base: 8px;
    --font: "Helvetica Neue", Helvetica, Arial, "Inter", sans-serif;
    /* Modular scale, ratio 1.25 */
    --step-0: 1rem;
    --step-1: 1.25rem;
    --step-2: 1.5625rem;
    --step-3: 1.953rem;
    --step-4: 2.441rem;
    --step-5: clamp(3rem, 7vw, 5.25rem);
  }

  * { box-sizing: border-box; }
  body {
    margin: 0;
    background: var(--paper);
    color: var(--ink);
    font-family: var(--font);
    font-size: var(--step-0);
    line-height: calc(var(--base) * 3);
  }

  .grid {
    display: grid;
    grid-template-columns: repeat(12, minmax(0, 1fr));
    column-gap: var(--gutter);
    max-width: 1280px;
    margin: 0 auto;
    padding: calc(var(--base) * 8) calc(var(--gutter) * 1.5);
  }

  .headline { grid-column: 1 / span 8; font-size: var(--step-5); line-height: 1; letter-spacing: -0.03em; font-weight: 700; margin: 0 0 calc(var(--base) * 6); }
  .intro { grid-column: 9 / span 4; font-size: var(--step-1); line-height: 1.5; margin: 0; }
  .rule { grid-column: 1 / -1; border: 0; border-top: 1px solid var(--ink); margin: calc(var(--base) * 6) 0; }

  .col-label { grid-column: 1 / span 3; font-size: 0.875rem; font-weight: 600; text-transform: uppercase; letter-spacing: 0.04em; color: var(--grey); }
  .col-body { grid-column: 4 / span 6; font-size: var(--step-0); max-width: 65ch; margin: 0; }
  .col-figure { grid-column: 10 / span 3; font-size: var(--step-3); font-weight: 700; color: var(--accent); margin: 0; }

  .cta {
    grid-column: 1 / span 3;
    display: inline-flex;
    align-items: center;
    min-height: 44px;
    padding: 0 var(--base) * 2;
    background: var(--ink);
    color: var(--paper);
    text-decoration: none;
    font-weight: 600;
  }
  .cta:hover { background: var(--accent); }
  .cta:focus-visible { outline: 2px solid var(--accent); outline-offset: 4px; }

  @media (max-width: 760px) {
    .headline, .intro, .col-label, .col-body, .col-figure, .cta { grid-column: 1 / -1; }
    .intro { margin-top: calc(var(--base) * 3); }
  }
</style>
</head>
<body>
  <main class="grid">
    <h1 class="headline">Order through the grid.</h1>
    <p class="intro">Every element sits on a visible system. Hierarchy comes from scale and position, not ornament.</p>

    <hr class="rule">

    <p class="col-label">Method</p>
    <p class="col-body">Set text flush left and ragged right. Align each block to column edges and build spacing on a single baseline.</p>
    <p class="col-figure">12</p>

    <hr class="rule">

    <a class="cta" href="#">Read the system</a>
  </main>
</body>
</html>
```

## Rules for implementation
- Use `grid-column` spans for every block; avoid ad hoc margins to position elements.
- Keep text flush left; never centre paragraphs.
- Keep the accent colour to one value and use it sparingly.
- Keep line length within 65ch for body copy.
