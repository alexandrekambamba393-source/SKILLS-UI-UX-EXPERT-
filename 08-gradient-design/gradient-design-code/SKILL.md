---
name: gradient-design-code
description: Production-ready HTML and CSS gradient system with oklch interpolation, a mesh hero background, a gradient button with scrim-protected text, a slow animated accent and reduced-motion fallback. Use when implementing gradient backgrounds, buttons and accents in plain HTML and CSS.
---

# Gradient Design: HTML and CSS Implementation

```html
<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Gradient System</title>
<style>
  :root {
    --ink: #11131a;
    --paper: #ffffff;
    --muted: #4a5160;
    --fallback: #3b5bdb;
    --grad-brand: linear-gradient(in oklch 135deg, oklch(62% 0.19 265), oklch(60% 0.22 300));
    --grad-mesh:
      radial-gradient(at 20% 20%, oklch(78% 0.14 265) 0, transparent 50%),
      radial-gradient(at 80% 30%, oklch(80% 0.12 230) 0, transparent 50%),
      radial-gradient(at 50% 90%, oklch(82% 0.13 300) 0, transparent 55%),
      oklch(96% 0.01 265);
  }

  * { box-sizing: border-box; }
  body { margin: 0; font-family: system-ui, sans-serif; color: var(--ink); background: var(--paper); }

  .hero {
    min-height: 70vh;
    display: grid;
    place-items: center;
    padding: 64px 24px;
    background: var(--fallback);
    background: var(--grad-mesh);
    position: relative;
    isolation: isolate;
  }

  .hero::after {
    content: "";
    position: absolute;
    inset: -20%;
    z-index: -1;
    background: var(--grad-brand);
    opacity: 0.12;
    filter: blur(60px);
    animation: drift 18s ease-in-out infinite alternate;
  }

  @keyframes drift {
    from { transform: translate3d(-4%, 0, 0) rotate(0deg); }
    to   { transform: translate3d(4%, -3%, 0) rotate(6deg); }
  }

  .card {
    max-width: 560px;
    background: rgba(255, 255, 255, 0.82);
    backdrop-filter: blur(8px);
    -webkit-backdrop-filter: blur(8px);
    border-radius: 20px;
    padding: 32px;
    box-shadow: 0 10px 30px rgba(17, 19, 26, 0.08);
  }
  .card h1 { margin: 0 0 12px; font-size: clamp(2rem, 5vw, 3rem); line-height: 1.1; letter-spacing: -0.02em; }
  .card p { margin: 0 0 20px; color: var(--muted); line-height: 1.6; }

  .btn {
    min-height: 44px;
    padding: 0 22px;
    border: 0;
    border-radius: 12px;
    background: var(--grad-brand);
    color: #ffffff;
    font: inherit;
    font-weight: 600;
    cursor: pointer;
    box-shadow: 0 6px 18px oklch(60% 0.2 280 / 0.35);
    transition: transform 150ms ease, box-shadow 150ms ease;
  }
  .btn:hover { transform: translateY(-1px); box-shadow: 0 10px 24px oklch(60% 0.2 280 / 0.45); }
  .btn:focus-visible { outline: 3px solid var(--ink); outline-offset: 3px; }

  @media (prefers-reduced-motion: reduce) {
    .hero::after { animation: none; }
    .btn { transition: none; }
  }
</style>
</head>
<body>
  <header class="hero">
    <section class="card">
      <h1>Gradients that guide the eye</h1>
      <p>Used for emphasis and atmosphere, with text kept on a calm surface.</p>
      <button class="btn">Start now</button>
    </section>
  </header>
</body>
</html>
```

## Rules for implementation
- Interpolate in `oklch` or `oklab` for clean midpoints; reserve plain RGB for simple accents.
- Keep text on a solid or scrimmed surface, not directly on saturated gradient areas.
- Always declare a solid fallback color before the gradient.
- Stop or slow decorative animation under `prefers-reduced-motion`.
