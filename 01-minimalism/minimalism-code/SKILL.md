---
name: minimalism-code
description: Production-ready HTML and CSS code for minimalist interfaces, including design tokens, an 8px spacing scale, a restrained button system and a clean content layout. Use when implementing a minimal UI in code.
---

# Minimalism: Implementation Code

## Design tokens and base layout
```html
<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Minimal UI</title>
<style>
  :root {
    --color-bg: #fafafa;
    --color-surface: #ffffff;
    --color-text: #111111;
    --color-muted: #6b6b6b;
    --color-border: #e6e6e6;
    --color-accent: #2563eb;
    --color-accent-text: #ffffff;

    --font-sans: "Inter", system-ui, -apple-system, "Segoe UI", sans-serif;
    --text-sm: 0.875rem;
    --text-base: 1rem;
    --text-lg: 1.25rem;
    --text-xl: 2rem;

    --space-1: 8px;
    --space-2: 16px;
    --space-3: 24px;
    --space-4: 32px;
    --space-6: 48px;
    --space-8: 64px;

    --radius: 8px;
    --ease: cubic-bezier(0.2, 0.6, 0.2, 1);
  }

  *, *::before, *::after { box-sizing: border-box; }
  html { -webkit-font-smoothing: antialiased; }
  body {
    margin: 0;
    font-family: var(--font-sans);
    font-size: var(--text-base);
    line-height: 1.6;
    color: var(--color-text);
    background: var(--color-bg);
  }

  .container {
    max-width: 640px;
    margin-inline: auto;
    padding: var(--space-8) var(--space-3);
    display: grid;
    gap: var(--space-4);
  }

  h1 { font-size: var(--text-xl); font-weight: 600; letter-spacing: -0.02em; margin: 0; }
  p  { margin: 0; color: var(--color-muted); }

  .btn {
    display: inline-flex;
    align-items: center;
    justify-content: center;
    min-height: 44px;
    padding: 0 var(--space-3);
    border: 0;
    border-radius: var(--radius);
    font: inherit;
    font-weight: 600;
    cursor: pointer;
    transition: background-color 180ms var(--ease), transform 120ms var(--ease);
  }
  .btn-primary { background: var(--color-accent); color: var(--color-accent-text); }
  .btn-primary:hover { background: #1d4ed8; }
  .btn-primary:active { transform: translateY(1px); }
  .btn-ghost { background: transparent; color: var(--color-text); }
  .btn-ghost:hover { background: var(--color-border); }

  .btn:focus-visible { outline: 2px solid var(--color-accent); outline-offset: 3px; }
  .btn:disabled { opacity: 0.4; cursor: not-allowed; }

  .actions { display: flex; gap: var(--space-2); flex-wrap: wrap; }

  @media (prefers-reduced-motion: reduce) {
    * { transition: none !important; animation: none !important; }
  }
</style>
</head>
<body>
  <main class="container">
    <header>
      <h1>One clear message</h1>
      <p>A single sentence that explains the value of this screen.</p>
    </header>
    <div class="actions">
      <button class="btn btn-primary">Get started</button>
      <button class="btn btn-ghost">Learn more</button>
    </div>
  </main>
</body>
</html>
```

## Rules for implementation
- Use only tokens from `:root`. Do not hardcode colors or spacing in components.
- Keep a single accent variable and never introduce a second one.
- Respect `prefers-reduced-motion`, as shown above.
- Keep touch targets at 44px or more.
