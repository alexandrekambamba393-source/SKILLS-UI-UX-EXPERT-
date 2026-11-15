---
name: glassmorphism-code
description: Production-ready glassmorphism code with backdrop blur panels, light edges, a layered gradient backdrop, a solid fallback and reduced-transparency support. Use when implementing frosted-glass UI components in CSS and HTML.
---

# Glassmorphism: Implementation Code

## Glass panels on a layered backdrop
```html
<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Glass UI</title>
<style>
  :root {
    --glass-fill: rgba(255, 255, 255, 0.14);
    --glass-fill-strong: rgba(255, 255, 255, 0.24);
    --glass-edge: rgba(255, 255, 255, 0.32);
    --glass-shadow: 0 8px 32px rgba(10, 14, 30, 0.25);
    --text: #ffffff;
    --text-muted: rgba(255, 255, 255, 0.82);
    --accent: #ffd166;
    --radius-lg: 20px;
    --radius-md: 14px;
  }

  * { box-sizing: border-box; }

  body {
    margin: 0;
    min-height: 100vh;
    font-family: system-ui, sans-serif;
    color: var(--text);
    background:
      radial-gradient(circle at 15% 20%, #7b5cff 0, transparent 40%),
      radial-gradient(circle at 85% 30%, #00c2ff 0, transparent 38%),
      radial-gradient(circle at 50% 90%, #ff6fa8 0, transparent 42%),
      #1b1f3b;
    display: grid;
    place-items: center;
    padding: 24px;
  }

  .glass {
    background: var(--glass-fill);
    border: 1px solid var(--glass-edge);
    box-shadow: var(--glass-shadow), inset 0 1px 0 rgba(255, 255, 255, 0.25);
    -webkit-backdrop-filter: blur(18px) saturate(140%);
    backdrop-filter: blur(18px) saturate(140%);
    border-radius: var(--radius-lg);
  }

  .panel {
    width: min(420px, 100%);
    padding: 28px;
    display: grid;
    gap: 16px;
  }
  .panel h2 { margin: 0; font-size: 1.5rem; }
  .panel p { margin: 0; color: var(--text-muted); line-height: 1.6; }

  .stat {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 14px 16px;
    border-radius: var(--radius-md);
    background: var(--glass-fill-strong);
    border: 1px solid var(--glass-edge);
  }
  .stat strong { font-size: 1.25rem; }

  .btn {
    min-height: 44px;
    padding: 0 18px;
    border: 0;
    border-radius: var(--radius-md);
    background: var(--accent);
    color: #1b1f3b;
    font: inherit;
    font-weight: 700;
    cursor: pointer;
    transition: transform 150ms ease, box-shadow 150ms ease;
  }
  .btn:hover { transform: translateY(-1px); box-shadow: 0 6px 20px rgba(255, 209, 102, 0.35); }
  .btn:focus-visible { outline: 3px solid #ffffff; outline-offset: 3px; }

  /* Fallback for browsers without backdrop-filter support */
  @supports not ((backdrop-filter: blur(1px)) or (-webkit-backdrop-filter: blur(1px))) {
    .glass { background: rgba(20, 24, 50, 0.85); }
  }

  /* Users who prefer less transparency get solid surfaces */
  @media (prefers-reduced-transparency: reduce) {
    .glass {
      background: #1f2547;
      -webkit-backdrop-filter: none;
      backdrop-filter: none;
    }
  }
</style>
</head>
<body>
  <section class="glass panel" aria-labelledby="balance-title">
    <h2 id="balance-title">Monthly balance</h2>
    <p>Your spending is 12 percent below the previous month.</p>
    <div class="stat"><span>Available</span><strong>$4,280</strong></div>
    <div class="stat"><span>Savings goal</span><strong>68%</strong></div>
    <button class="btn">Review budget</button>
  </section>
</body>
</html>
```

## Rules for implementation
- Keep blur between 12px and 24px; reserve saturation boosts for backgrounds with color.
- Limit nested glass layers to two.
- Always provide the `@supports` fallback and the reduced-transparency rule.
- Verify text contrast against the lightest and darkest backdrop areas.
