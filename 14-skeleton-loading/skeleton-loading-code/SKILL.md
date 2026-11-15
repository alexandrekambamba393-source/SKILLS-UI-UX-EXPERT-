---
name: skeleton-loading-code
description: Production-ready HTML and CSS skeleton loading with a shimmer animation, a card skeleton matching a real layout, a delayed-show pattern, aria-busy and reduced-motion support. Use when implementing placeholder loading states in plain HTML and CSS.
---

# Skeleton Loading: HTML and CSS Implementation

```html
<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Loading Articles</title>
<style>
  :root {
    --base: #e6e8ec;
    --highlight: #f3f4f6;
    --card: #ffffff;
    --line: #e4e7ec;
    --ink: #111418;
    --muted: #5b6470;
  }

  * { box-sizing: border-box; }
  body { margin: 0; font-family: system-ui, sans-serif; background: #f5f6f8; color: var(--ink); }

  .feed { max-width: 720px; margin: 48px auto; padding: 0 20px; display: grid; gap: 20px; }

  .card { background: var(--card); border: 1px solid var(--line); border-radius: 16px; padding: 20px; display: grid; gap: 14px; }

  .skeleton {
    background: linear-gradient(90deg, var(--base) 0%, var(--highlight) 50%, var(--base) 100%);
    background-size: 200% 100%;
    border-radius: 8px;
    animation: shimmer 1.6s ease-in-out infinite;
  }
  .sk-avatar { width: 44px; height: 44px; border-radius: 50%; }
  .sk-line { height: 14px; }
  .sk-title { height: 22px; width: 70%; }
  .sk-image { height: 180px; border-radius: 12px; }
  .sk-line.w-92 { width: 92%; }
  .sk-line.w-70 { width: 70%; }

  .row { display: flex; align-items: center; gap: 12px; }

  /* Skeleton appears only after a short delay so fast loads do not flash */
  .delayed { animation: shimmer 1.6s ease-in-out infinite, show 0s 200ms both; }

  .real { opacity: 0; transition: opacity 180ms ease-out; }
  .real.is-loaded { opacity: 1; }

  @keyframes shimmer {
    0% { background-position: 100% 0; }
    100% { background-position: -100% 0; }
  }
  @keyframes show { to { visibility: visible; } }
  .delayed { visibility: hidden; }

  @media (prefers-reduced-motion: reduce) {
    .skeleton, .delayed { animation: none; background: var(--base); }
  }
  .sr-only { position: absolute; width: 1px; height: 1px; overflow: hidden; clip: rect(0 0 0 0); white-space: nowrap; }
</style>
</head>
<body>
  <main class="feed" id="feed" aria-busy="true" aria-describedby="status">
    <p id="status" class="sr-only" role="status">Loading articles</p>

    <div class="card delayed" aria-hidden="true">
      <div class="row">
        <div class="skeleton sk-avatar"></div>
        <div class="skeleton sk-line" style="width:40%"></div>
      </div>
      <div class="skeleton sk-title"></div>
      <div class="skeleton sk-line w-92"></div>
      <div class="skeleton sk-line w-70"></div>
      <div class="skeleton sk-image"></div>
    </div>

    <div class="card delayed" aria-hidden="true">
      <div class="row">
        <div class="skeleton sk-avatar"></div>
        <div class="skeleton sk-line" style="width:40%"></div>
      </div>
      <div class="skeleton sk-title"></div>
      <div class="skeleton sk-line w-92"></div>
      <div class="skeleton sk-line w-70"></div>
    </div>
  </main>

<script>
  // Simulated fetch: replace with your real request.
  const feed = document.getElementById("feed");
  const status = document.getElementById("status");

  setTimeout(() => {
    feed.setAttribute("aria-busy", "false");
    status.textContent = "Articles loaded";
    feed.innerHTML = `
      <article class="card real is-loaded">
        <div class="row"><strong>Maya Lin</strong><span style="color:#5b6470">2 hours ago</span></div>
        <h2 style="margin:0;font-size:1.25rem">How teams ship smaller releases</h2>
        <p style="margin:0;color:#5b6470;line-height:1.6">Breaking work into smaller increments reduces risk and speeds up feedback.</p>
      </article>`;
  }, 1800);
</script>
</body>
</html>
```

## Rules for implementation
- Match skeleton blocks to the real layout: same card structure and rough line counts.
- Use `aria-busy` on the container and a visually hidden status for screen readers; hide the decorative blocks with `aria-hidden`.
- Delay visibility by about 200ms so fast loads do not flash placeholders.
- Replace the shimmer with a static block under reduced motion.
