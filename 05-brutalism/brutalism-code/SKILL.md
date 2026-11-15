---
name: brutalism-code
description: Production-ready HTML and CSS neo-brutalist components with hard offset shadows, thick borders, oversized headings, interactive press states and a readable article layout. Use when implementing a bold brutalist interface in plain HTML and CSS.
---

# Brutalism: HTML and CSS Implementation

```html
<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Brutalist Landing</title>
<style>
  :root {
    --paper: #f4f1ea;
    --ink: #000000;
    --accent-1: #ffd23f;
    --accent-2: #ff5c5c;
    --border: 3px solid var(--ink);
    --shadow: 6px 6px 0 var(--ink);
    --font-display: "Archivo Black", "Arial Black", sans-serif;
    --font-body: "Inter", system-ui, sans-serif;
  }

  * { box-sizing: border-box; }
  body {
    margin: 0;
    background: var(--paper);
    color: var(--ink);
    font-family: var(--font-body);
    font-size: 1rem;
    line-height: 1.6;
  }

  .wrap { max-width: 1100px; margin: 0 auto; padding: 48px 24px; display: grid; gap: 40px; }

  h1 {
    font-family: var(--font-display);
    font-size: clamp(3rem, 9vw, 6rem);
    line-height: 0.95;
    letter-spacing: -0.03em;
    margin: 0;
    text-transform: uppercase;
  }

  .card {
    background: #ffffff;
    border: var(--border);
    box-shadow: var(--shadow);
    padding: 24px;
  }
  .card h2 { margin: 0 0 8px; font-size: 1.5rem; }
  .card p { margin: 0; }

  .grid { display: grid; gap: 32px; grid-template-columns: repeat(auto-fit, minmax(240px, 1fr)); }

  .btn {
    display: inline-flex;
    align-items: center;
    min-height: 48px;
    padding: 0 22px;
    border: var(--border);
    background: var(--accent-1);
    color: var(--ink);
    font: inherit;
    font-weight: 800;
    text-transform: uppercase;
    letter-spacing: 0.02em;
    box-shadow: var(--shadow);
    cursor: pointer;
    transition: transform 100ms ease, box-shadow 100ms ease;
  }
  .btn:hover { transform: translate(-2px, -2px); box-shadow: 8px 8px 0 var(--ink); }
  .btn:active { transform: translate(6px, 6px); box-shadow: none; }
  .btn:focus-visible { outline: 4px solid var(--accent-2); outline-offset: 4px; }

  .tag {
    display: inline-block;
    padding: 4px 10px;
    border: 2px solid var(--ink);
    background: var(--accent-2);
    font-weight: 700;
    font-size: 0.875rem;
  }

  @media (prefers-reduced-motion: reduce) { .btn { transition: none; } }
</style>
</head>
<body>
  <main class="wrap">
    <header>
      <span class="tag">Studio 2026</span>
      <h1>Build loud. Ship raw.</h1>
    </header>

    <div class="grid">
      <article class="card">
        <h2>Fast</h2>
        <p>Pages load in under one second on a standard connection.</p>
      </article>
      <article class="card">
        <h2>Honest</h2>
        <p>Every feature is visible. Nothing is hidden behind menus.</p>
      </article>
    </div>

    <div>
      <button class="btn">Start a project</button>
    </div>
  </main>
</body>
</html>
```

## Rules for implementation
- Use `0` radius and hard shadows (`0` blur). Never introduce blurred shadows into this system.
- Keep the accent palette to two colours plus ink and paper.
- Make press states mechanical: the element moves into its shadow offset.
- Keep body text in a regular weight and at least 16px.
