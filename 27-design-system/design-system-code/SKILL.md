---
name: design-system-code
description: Production-ready HTML and CSS design system foundation with three-tier design tokens, light and dark theme switching through semantic token overrides, a button component with complete states and a card component built only from semantic tokens. Use when establishing a token-based design system in plain HTML and CSS.
---

# Design System: HTML and CSS Implementation

```html
<!doctype html>
<html lang="en" data-theme="light">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Design System Foundation</title>
<style>
  /* 1. Primitive tokens: raw values, never used directly by components */
  :root {
    --blue-600: #2f5bea;
    --blue-700: #2448c4;
    --blue-300: #a9bff9;
    --gray-900: #101828;
    --gray-600: #475467;
    --gray-300: #d0d5dd;
    --gray-50: #f7f8fa;
    --white: #ffffff;
    --red-600: #b42318;
    --space-1: 4px; --space-2: 8px; --space-3: 12px; --space-4: 16px; --space-6: 24px; --space-8: 32px;
    --radius-sm: 8px; --radius-md: 12px; --radius-lg: 16px;
    --font-sans: "Inter", system-ui, sans-serif;
    --motion-fast: 150ms; --motion-base: 220ms;
  }

  /* 2. Semantic tokens: intent-based, the only layer components read */
  :root[data-theme="light"] {
    --color-bg: var(--gray-50);
    --color-surface: var(--white);
    --color-text: var(--gray-900);
    --color-text-muted: var(--gray-600);
    --color-border: var(--gray-300);
    --color-action: var(--blue-600);
    --color-action-hover: var(--blue-700);
    --color-action-text: var(--white);
    --color-focus: var(--blue-600);
    --color-danger: var(--red-600);
  }

  :root[data-theme="dark"] {
    --color-bg: #0e1116;
    --color-surface: #161b22;
    --color-text: #e8ebef;
    --color-text-muted: #9aa3ae;
    --color-border: #2c343f;
    --color-action: #6aa0ff;
    --color-action-hover: #8fb8ff;
    --color-action-text: #0b1220;
    --color-focus: #8fb8ff;
    --color-danger: #ff8a80;
  }

  /* 3. Component tokens: reference semantic tokens only */
  :root {
    --button-bg: var(--color-action);
    --button-bg-hover: var(--color-action-hover);
    --button-text: var(--color-action-text);
    --card-bg: var(--color-surface);
    --card-border: var(--color-border);
  }

  * { box-sizing: border-box; }
  body {
    margin: 0;
    min-height: 100vh;
    display: grid;
    place-items: center;
    padding: var(--space-6);
    font-family: var(--font-sans);
    background: var(--color-bg);
    color: var(--color-text);
    transition: background-color var(--motion-base) ease, color var(--motion-base) ease;
  }

  /* Button component */
  .btn {
    min-height: 44px;
    padding: 0 var(--space-6);
    border: 1px solid transparent;
    border-radius: var(--radius-sm);
    background: var(--button-bg);
    color: var(--button-text);
    font: inherit;
    font-weight: 600;
    cursor: pointer;
    transition: background-color var(--motion-fast) ease, transform var(--motion-fast) ease;
  }
  .btn:hover { background: var(--button-bg-hover); }
  .btn:active { transform: translateY(1px); }
  .btn:focus-visible { outline: 3px solid var(--color-focus); outline-offset: 3px; }
  .btn:disabled { opacity: 0.5; cursor: not-allowed; }
  .btn[aria-busy="true"] { cursor: progress; }
  .btn-secondary { background: transparent; color: var(--color-text); border-color: var(--color-border); }
  .btn-secondary:hover { background: var(--color-bg); }

  /* Card component */
  .card {
    width: min(360px, 100%);
    background: var(--card-bg);
    border: 1px solid var(--card-border);
    border-radius: var(--radius-lg);
    padding: var(--space-6);
    display: grid;
    gap: var(--space-4);
  }
  .card h2 { margin: 0; font-size: 1.25rem; }
  .card p { margin: 0; color: var(--color-text-muted); line-height: 1.6; }
  .card .actions { display: flex; gap: var(--space-3); flex-wrap: wrap; }

  .theme-toggle { position: fixed; top: 16px; right: 16px; }

  @media (prefers-reduced-motion: reduce) {
    *, *::before, *::after { transition: none !important; }
  }
</style>
</head>
<body>
  <button class="btn btn-secondary theme-toggle" id="theme" type="button" aria-pressed="false">Dark theme</button>

  <article class="card" aria-labelledby="card-title">
    <h2 id="card-title">Project summary</h2>
    <p>Tokens drive every colour and spacing decision. Switch themes to see semantic values change without touching components.</p>
    <div class="actions">
      <button class="btn" type="button">Primary action</button>
      <button class="btn btn-secondary" type="button">Secondary</button>
    </div>
  </article>

<script>
  const root = document.documentElement;
  const toggle = document.getElementById("theme");
  const saved = (() => { try { return localStorage.getItem("ds-theme"); } catch { return null; } })();
  if (saved) root.setAttribute("data-theme", saved);

  function sync() {
    const dark = root.getAttribute("data-theme") === "dark";
    toggle.setAttribute("aria-pressed", String(dark));
    toggle.textContent = dark ? "Light theme" : "Dark theme";
  }
  toggle.addEventListener("click", () => {
    const next = root.getAttribute("data-theme") === "dark" ? "light" : "dark";
    root.setAttribute("data-theme", next);
    try { localStorage.setItem("ds-theme", next); } catch {}
    sync();
  });
  sync();
</script>
</body>
</html>
```

## Rules for implementation
- Components read only semantic or component tokens, never primitives.
- Theme by overriding semantic tokens under `data-theme`, not by restyling components.
- Build every component with its full set of states, including focus, disabled and busy.
- Keep spacing, radius and motion durations as named tokens.
