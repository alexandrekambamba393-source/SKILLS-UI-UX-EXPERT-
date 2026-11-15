---
name: responsive-design-code
description: Production-ready HTML and CSS responsive layout using mobile-first breakpoints, fluid typography with clamp(), a responsive header with a menu toggle, an auto-fit card grid, reflow-safe content and touch-target sizing. Use when implementing a responsive page layout in plain HTML and CSS.
---

# Responsive Design: HTML and CSS Implementation

```html
<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Responsive Layout</title>
<style>
  :root {
    --ink: #111418;
    --muted: #4a5160;
    --line: #e4e7ec;
    --accent: #3b5bdb;
    --surface: #ffffff;
    --bg: #f5f6f8;
    --gutter: clamp(16px, 4vw, 40px);
    --step-0: clamp(1rem, 0.95rem + 0.25vw, 1.125rem);
    --step-2: clamp(1.5rem, 1.2rem + 1.5vw, 2.25rem);
    --step-3: clamp(2rem, 1.4rem + 2.5vw, 3.25rem);
  }

  * { box-sizing: border-box; }
  body { margin: 0; font-family: system-ui, sans-serif; font-size: var(--step-0); color: var(--ink); background: var(--bg); line-height: 1.6; }
  img, svg { display: block; max-width: 100%; height: auto; }

  .skip { position: absolute; left: 12px; top: -48px; background: var(--ink); color: #fff; padding: 8px 12px; border-radius: 8px; z-index: 10; }
  .skip:focus { top: 12px; }

  header.site { background: var(--surface); border-bottom: 1px solid var(--line); position: sticky; top: 0; z-index: 5; }
  .bar { display: flex; align-items: center; justify-content: space-between; gap: 16px; padding: 12px var(--gutter); min-height: 64px; }
  .brand { font-weight: 700; text-decoration: none; color: var(--ink); }

  .menu-toggle {
    min-width: 44px; min-height: 44px; border: 1px solid var(--line); border-radius: 10px; background: var(--surface); cursor: pointer; font: inherit; font-weight: 600;
  }
  .menu-toggle:focus-visible, a:focus-visible, button:focus-visible { outline: 3px solid var(--accent); outline-offset: 3px; }

  nav.primary { display: none; }
  nav.primary.open { display: grid; padding: 8px var(--gutter) 16px; gap: 4px; border-top: 1px solid var(--line); }
  nav.primary a { display: flex; align-items: center; min-height: 44px; padding: 0 8px; color: var(--ink); text-decoration: none; border-radius: 8px; }
  nav.primary a:hover { background: var(--bg); }

  main { padding: 32px var(--gutter) 64px; max-width: 1200px; margin: 0 auto; }
  h1 { font-size: var(--step-3); line-height: 1.1; letter-spacing: -0.02em; margin: 0 0 16px; max-width: 22ch; }
  .lede { color: var(--muted); max-width: 60ch; margin: 0 0 32px; }

  .cards { display: grid; gap: 20px; grid-template-columns: repeat(auto-fit, minmax(min(100%, 260px), 1fr)); }
  .card { background: var(--surface); border: 1px solid var(--line); border-radius: 16px; padding: 24px; }
  .card h2 { font-size: var(--step-2); margin: 0 0 8px; line-height: 1.2; }
  .card p { margin: 0; color: var(--muted); }

  .cta {
    display: inline-flex; align-items: center; justify-content: center; min-height: 48px; padding: 0 22px;
    margin-top: 32px; background: var(--accent); color: #fff; border-radius: 12px; text-decoration: none; font-weight: 600;
  }

  /* Tablet and up */
  @media (min-width: 768px) {
    nav.primary { display: flex; gap: 8px; border: 0; padding: 0; }
    nav.primary a { min-height: 40px; }
    .menu-toggle { display: none; }
  }

  /* Coarse pointers get larger targets */
  @media (pointer: coarse) {
    .cta, nav.primary a { min-height: 48px; }
  }

  @media (prefers-reduced-motion: reduce) { * { scroll-behavior: auto !important; } }
</style>
</head>
<body>
  <a class="skip" href="#main">Skip to content</a>

  <header class="site">
    <div class="bar">
      <a class="brand" href="#">Northwind</a>
      <button class="menu-toggle" type="button" aria-expanded="false" aria-controls="primary-nav" id="toggle">Menu</button>
    </div>
    <nav class="primary" id="primary-nav" aria-label="Primary">
      <a href="#products">Products</a>
      <a href="#pricing">Pricing</a>
      <a href="#support">Support</a>
    </nav>
  </header>

  <main id="main">
    <h1>Software that adapts to every screen</h1>
    <p class="lede">Content reflows by priority. The primary action stays reachable on phones, tablets and large displays alike.</p>

    <section class="cards" aria-label="Features">
      <article class="card"><h2>Fast setup</h2><p>Get your team running in minutes, from any device.</p></article>
      <article class="card"><h2>Shared views</h2><p>Dashboards keep the same information on every screen size.</p></article>
      <article class="card"><h2>Offline access</h2><p>Work continues when the connection is unstable.</p></article>
    </section>

    <a class="cta" href="#start">Start free trial</a>
  </main>

<script>
  const toggle = document.getElementById("toggle");
  const nav = document.getElementById("primary-nav");
  toggle.addEventListener("click", () => {
    const open = nav.classList.toggle("open");
    toggle.setAttribute("aria-expanded", String(open));
  });
</script>
</body>
</html>
```

## Rules for implementation
- Write base styles for the smallest screen and add `min-width` breakpoints upward.
- Use `clamp()` for fluid type and spacing; keep body text at 16px or more.
- Use `auto-fit` with `minmax(min(100%, 260px), 1fr)` so cards never overflow on narrow screens.
- Enlarge targets for coarse pointers and keep them at 44px minimum.
