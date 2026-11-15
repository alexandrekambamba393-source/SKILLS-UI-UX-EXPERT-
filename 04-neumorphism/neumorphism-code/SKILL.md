---
name: neumorphism-code
description: Production-ready HTML and CSS neumorphic components with raised and inset states, a toggle switch, a pressable button, accessible focus rings and a reduced-motion fallback. Use when implementing soft-relief UI controls in plain HTML and CSS.
---

# Neumorphism: HTML and CSS Implementation

```html
<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Soft UI Controls</title>
<style>
  :root {
    --base: #e0e5ec;
    --text: #2d3440;
    --muted: #5a6478;
    --accent: #4f6ef7;
    --shadow-dark: rgba(163, 177, 198, 0.7);
    --shadow-light: rgba(255, 255, 255, 0.85);
    --radius: 16px;
  }

  * { box-sizing: border-box; }
  body {
    margin: 0;
    min-height: 100vh;
    display: grid;
    place-items: center;
    background: var(--base);
    color: var(--text);
    font-family: system-ui, sans-serif;
  }

  .panel {
    background: var(--base);
    border-radius: 24px;
    padding: 32px;
    width: min(360px, 92vw);
    display: grid;
    gap: 24px;
    box-shadow: 10px 10px 20px var(--shadow-dark), -10px -10px 20px var(--shadow-light);
  }

  .raised {
    background: var(--base);
    border: 0;
    border-radius: var(--radius);
    min-height: 44px;
    padding: 0 20px;
    font: inherit;
    font-weight: 600;
    color: var(--text);
    cursor: pointer;
    box-shadow: 6px 6px 12px var(--shadow-dark), -6px -6px 12px var(--shadow-light);
    transition: box-shadow 200ms ease, transform 120ms ease;
  }
  .raised:hover { box-shadow: 8px 8px 16px var(--shadow-dark), -8px -8px 16px var(--shadow-light); }
  .raised:active {
    box-shadow: inset 4px 4px 8px var(--shadow-dark), inset -4px -4px 8px var(--shadow-light);
    transform: translateY(1px);
  }
  .raised:focus-visible { outline: 2px solid var(--accent); outline-offset: 4px; }
  .raised:disabled { opacity: 0.5; cursor: not-allowed; box-shadow: none; }

  .toggle {
    position: relative;
    display: inline-flex;
    align-items: center;
    width: 64px;
    height: 34px;
    border-radius: 999px;
    background: var(--base);
    box-shadow: inset 4px 4px 8px var(--shadow-dark), inset -4px -4px 8px var(--shadow-light);
    cursor: pointer;
  }
  .toggle input { position: absolute; opacity: 0; inset: 0; margin: 0; cursor: pointer; }
  .toggle .knob {
    width: 26px; height: 26px; margin-left: 4px;
    border-radius: 50%;
    background: var(--base);
    box-shadow: 3px 3px 6px var(--shadow-dark), -3px -3px 6px var(--shadow-light);
    transition: transform 250ms ease;
  }
  .toggle input:checked + .knob { transform: translateX(30px); background: var(--accent); }
  .toggle:focus-within { outline: 2px solid var(--accent); outline-offset: 4px; }

  .label-row { display: flex; justify-content: space-between; align-items: center; color: var(--muted); }

  @media (prefers-reduced-motion: reduce) {
    .raised, .toggle .knob { transition: none; }
  }
</style>
</head>
<body>
  <section class="panel" aria-label="Device settings">
    <div class="label-row">
      <span id="wifi-label">Wi-Fi</span>
      <label class="toggle">
        <input type="checkbox" aria-labelledby="wifi-label" checked>
        <span class="knob" aria-hidden="true"></span>
      </label>
    </div>
    <button class="raised">Save changes</button>
    <button class="raised" disabled>Unavailable</button>
  </section>
</body>
</html>
```

## Rules for implementation
- Define both shadow colours as variables so the whole set can be retuned together.
- Keep one background variable; never introduce a second surface colour.
- The toggle uses a real checkbox for keyboard and screen reader support.
- Add a visible border or accent indicator if shadow contrast alone fails 3:1.
