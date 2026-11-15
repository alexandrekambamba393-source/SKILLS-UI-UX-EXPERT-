---
name: hover-effects-code
description: Production-ready HTML and CSS hover effects for cards, buttons, links and list rows, with hover-capable media queries, matching focus-visible states, touch :active feedback and reduced-motion support. Use when implementing accessible hover interactions in plain HTML and CSS.
---

# Hover Effects: HTML and CSS Implementation

```html
<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Hover Effects</title>
<style>
  :root {
    --ink: #111418;
    --muted: #5b6470;
    --accent: #3b5bdb;
    --line: #e4e7ec;
    --surface: #ffffff;
    --tint: #f1f4fb;
    --ease-in: cubic-bezier(0.4, 0, 1, 1);
    --ease-out: cubic-bezier(0.2, 0.6, 0.2, 1);
  }

  * { box-sizing: border-box; }
  body { margin: 0; font-family: system-ui, sans-serif; color: var(--ink); background: #f5f6f8; }

  .wrap { max-width: 960px; margin: 48px auto; padding: 0 24px; display: grid; gap: 28px; }

  /* Card lift only on hover-capable devices */
  .card {
    background: var(--surface);
    border: 1px solid var(--line);
    border-radius: 16px;
    padding: 24px;
    transition: transform 180ms var(--ease-out), box-shadow 220ms var(--ease-in);
  }
  @media (hover: hover) and (prefers-reduced-motion: no-preference) {
    .card:hover { transform: translateY(-3px); box-shadow: 0 12px 28px rgba(17, 20, 24, 0.10); }
  }
  .card:focus-within { outline: 2px solid var(--accent); outline-offset: 3px; }
  .card:active { transform: translateY(-1px); }
  .card h2 { margin: 0 0 8px; font-size: 1.125rem; }
  .card p { margin: 0; color: var(--muted); line-height: 1.6; }
  .card a.cover { color: inherit; text-decoration: none; }
  .card a.cover::after { content: ""; position: absolute; inset: 0; border-radius: inherit; }
  .card { position: relative; }
  .card a.cover:focus-visible { outline: none; }

  /* Text link: underline draws in on hover and focus */
  .link {
    color: var(--accent);
    text-decoration: none;
    background-image: linear-gradient(currentColor, currentColor);
    background-size: 0% 1px;
    background-repeat: no-repeat;
    background-position: 0 100%;
    transition: background-size 200ms var(--ease-out);
    padding-bottom: 2px;
  }
  .link:hover, .link:focus-visible { background-size: 100% 1px; }
  .link:focus-visible { outline: 2px solid var(--accent); outline-offset: 3px; }

  /* Button: tint on hover, matching focus, stronger press */
  .btn {
    min-height: 44px;
    padding: 0 18px;
    border: 1px solid #cfd6e4;
    border-radius: 10px;
    background: var(--surface);
    color: var(--ink);
    font: inherit;
    font-weight: 600;
    cursor: pointer;
    transition: background-color 150ms var(--ease-out), border-color 150ms var(--ease-out), transform 100ms var(--ease-out);
  }
  .btn:hover { background: var(--tint); border-color: var(--accent); }
  .btn:focus-visible { outline: 2px solid var(--accent); outline-offset: 3px; }
  .btn:active { transform: scale(0.98); }

  /* List row: tint on hover, reveal action on hover AND focus */
  .row {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 14px 16px;
    border-radius: 12px;
    background: var(--surface);
    transition: background-color 150ms var(--ease-out);
  }
  .row:hover { background: var(--tint); }
  .row .action {
    opacity: 0.55;
    font-weight: 600;
    color: var(--accent);
    background: none;
    border: 0;
    font: inherit;
    cursor: pointer;
    min-height: 40px;
    padding: 0 8px;
    transition: opacity 150ms var(--ease-out);
  }
  .row:hover .action, .row:focus-within .action, .action:focus-visible { opacity: 1; }
  .action:focus-visible { outline: 2px solid var(--accent); outline-offset: 2px; border-radius: 6px; }

  @media (prefers-reduced-motion: reduce) {
    .card, .btn, .link, .row, .row .action { transition: none; }
    .card:hover { transform: none; }
  }
</style>
</head>
<body>
  <main class="wrap">
    <article class="card">
      <h2><a class="cover" href="#analytics">Analytics overview</a></h2>
      <p>Track adoption, retention and revenue in one place.</p>
    </article>

    <p style="margin:0"><a class="link" href="#docs">Read the documentation</a></p>

    <div>
      <button class="btn" type="button">Invite a teammate</button>
    </div>

    <section aria-label="Projects">
      <div class="row">
        <span>Website redesign</span>
        <button class="action" type="button">Open</button>
      </div>
      <div class="row">
        <span>Mobile release</span>
        <button class="action" type="button">Open</button>
      </div>
    </section>
  </main>
</body>
</html>
```

## Rules for implementation
- Wrap lift and shadow effects in `(hover: hover)` so touch devices do not get sticky hover states.
- Give every hover effect a matching `:focus-visible` or `:focus-within` state.
- Show revealed actions on hover and on keyboard focus, never on hover alone.
- Disable non-essential transitions under `prefers-reduced-motion`.
