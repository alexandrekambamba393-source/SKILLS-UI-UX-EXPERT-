---
name: bento-grid-code
description: Production-ready HTML and CSS bento grid with a 4-column CSS grid, 1x1, 2x1, 1x2 and 2x2 tile spans, a hero tile, responsive reflow and keyboard-accessible links. Use when implementing a bento-style modular layout in plain HTML and CSS.
---

# Bento Grid: HTML and CSS Implementation

```html
<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Bento Feature Grid</title>
<style>
  :root {
    --bg: #f5f6f8;
    --tile: #ffffff;
    --ink: #111418;
    --muted: #5b6470;
    --accent: #3b5bdb;
    --gap: 20px;
    --radius: 20px;
    --pad: 24px;
  }

  * { box-sizing: border-box; }
  body { margin: 0; background: var(--bg); color: var(--ink); font-family: system-ui, sans-serif; }

  .bento {
    max-width: 1100px;
    margin: 48px auto;
    padding: 0 24px;
    display: grid;
    grid-template-columns: repeat(4, minmax(0, 1fr));
    grid-auto-rows: minmax(200px, auto);
    gap: var(--gap);
  }

  .tile {
    position: relative;
    background: var(--tile);
    border-radius: var(--radius);
    padding: var(--pad);
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    border: 1px solid rgba(17, 20, 24, 0.06);
    overflow: hidden;
    transition: transform 200ms ease, box-shadow 200ms ease;
  }
  .tile:hover { transform: translateY(-2px); box-shadow: 0 12px 28px rgba(17, 20, 24, 0.08); }

  .tile h2 { margin: 0 0 8px; font-size: 1.25rem; }
  .tile p { margin: 0; color: var(--muted); line-height: 1.5; }

  .span-2x1 { grid-column: span 2; }
  .span-1x2 { grid-row: span 2; }
  .span-2x2 { grid-column: span 2; grid-row: span 2; }

  .hero { background: linear-gradient(135deg, #3b5bdb, #7048e8); color: #ffffff; }
  .hero p { color: rgba(255, 255, 255, 0.85); }
  .metric { font-size: 2.75rem; font-weight: 700; letter-spacing: -0.02em; }

  .tile a.cover {
    position: absolute;
    inset: 0;
    z-index: 1;
    border-radius: inherit;
  }
  .tile a.cover:focus-visible { outline: 3px solid var(--accent); outline-offset: 2px; }

  @media (max-width: 900px) {
    .bento { grid-template-columns: repeat(2, minmax(0, 1fr)); }
    .span-2x2 { grid-column: span 2; grid-row: span 1; }
  }
  @media (max-width: 560px) {
    .bento { grid-template-columns: 1fr; }
    .span-2x1, .span-1x2, .span-2x2 { grid-column: auto; grid-row: auto; }
  }
  @media (prefers-reduced-motion: reduce) { .tile { transition: none; } .tile:hover { transform: none; } }
</style>
</head>
<body>
  <main class="bento">
    <article class="tile hero span-2x2">
      <div>
        <h2>Unified analytics</h2>
        <p>Every key metric in one view, updated in real time across your teams.</p>
      </div>
      <div class="metric">98.4%</div>
      <a class="cover" href="#analytics" aria-label="Open analytics"></a>
    </article>

    <article class="tile">
      <h2>Alerts</h2>
      <p>Get notified only when it matters.</p>
      <a class="cover" href="#alerts" aria-label="Open alerts"></a>
    </article>

    <article class="tile span-2x1">
      <h2>Team activity</h2>
      <p>See who changed what, and when, without digging through logs.</p>
      <a class="cover" href="#activity" aria-label="Open team activity"></a>
    </article>

    <article class="tile">
      <h2>Exports</h2>
      <p>CSV, PDF and API access.</p>
      <a class="cover" href="#exports" aria-label="Open exports"></a>
    </article>
  </main>
</body>
</html>
```

## Rules for implementation
- Assign spans with classes rather than inline styles, so responsive rules can override them.
- Use a single full-tile link (`.cover`) for a larger hit area; keep the link text accessible through `aria-label`.
- Keep DOM order equal to priority order so screen readers and keyboard users follow the same sequence.
- Keep gap and radius as variables so the whole grid stays consistent.
