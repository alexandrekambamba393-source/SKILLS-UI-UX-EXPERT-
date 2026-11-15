---
name: dark-mode-code
description: Production-ready dark mode code with CSS custom property theming, system preference detection, a persistent manual toggle and flash-free initialization. Use when implementing a light and dark theme in a web interface.
---

# Dark Mode: Implementation Code

## Theme tokens, no-flash script and toggle
```html
<!doctype html>
<html lang="en" data-theme="">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Dark Mode UI</title>
<script>
  // Runs before first paint to avoid a flash of the wrong theme.
  (function () {
    var saved = null;
    try { saved = localStorage.getItem("theme"); } catch (e) {}
    var theme = saved || (window.matchMedia("(prefers-color-scheme: dark)").matches ? "dark" : "light");
    document.documentElement.setAttribute("data-theme", theme);
  })();
</script>
<style>
  :root {
    color-scheme: light dark;
    --bg: #ffffff;
    --surface: #f5f6f8;
    --surface-raised: #ffffff;
    --border: #d9dde3;
    --text: #111418;
    --text-muted: #5b6470;
    --accent: #2f6fe4;
    --accent-text: #ffffff;
    --success: #1f8f55;
    --danger: #c73a3a;
    --focus: #2f6fe4;
  }

  [data-theme="dark"] {
    --bg: #0e1116;
    --surface: #161b22;
    --surface-raised: #1e242c;
    --border: #2c343f;
    --text: #e8ebef;
    --text-muted: #9aa3ae;
    --accent: #6aa0ff;
    --accent-text: #0b1220;
    --success: #5cc38a;
    --danger: #ff7a7a;
    --focus: #8fb8ff;
  }

  body {
    margin: 0;
    font-family: system-ui, sans-serif;
    background: var(--bg);
    color: var(--text);
    transition: background-color 200ms ease, color 200ms ease;
  }

  .card {
    background: var(--surface-raised);
    border: 1px solid var(--border);
    border-radius: 12px;
    padding: 24px;
    max-width: 420px;
    margin: 48px auto;
  }
  .card h2 { margin: 0 0 8px; }
  .card p { margin: 0 0 16px; color: var(--text-muted); }

  .btn {
    min-height: 44px;
    padding: 0 16px;
    border: 0;
    border-radius: 8px;
    background: var(--accent);
    color: var(--accent-text);
    font: inherit;
    font-weight: 600;
    cursor: pointer;
  }
  .btn:focus-visible { outline: 3px solid var(--focus); outline-offset: 3px; }

  .status-ok { color: var(--success); }
  .status-error { color: var(--danger); }

  img.photo { filter: brightness(0.9); }
  [data-theme="dark"] img.photo { filter: brightness(0.85); }
</style>
</head>
<body>
  <main class="card">
    <h2>Account overview</h2>
    <p>Theme follows your system setting until you choose one.</p>
    <p class="status-ok">Payments are up to date.</p>
    <button class="btn" id="theme-toggle" aria-pressed="false">Switch theme</button>
  </main>

<script>
  (function () {
    var root = document.documentElement;
    var btn = document.getElementById("theme-toggle");

    function apply(theme) {
      root.setAttribute("data-theme", theme);
      btn.setAttribute("aria-pressed", String(theme === "dark"));
    }

    apply(root.getAttribute("data-theme"));

    btn.addEventListener("click", function () {
      var next = root.getAttribute("data-theme") === "dark" ? "light" : "dark";
      apply(next);
      try { localStorage.setItem("theme", next); } catch (e) {}
    });

    // Follow system changes only when the user has not chosen a theme.
    window.matchMedia("(prefers-color-scheme: dark)").addEventListener("change", function (e) {
      var saved = null;
      try { saved = localStorage.getItem("theme"); } catch (err) {}
      if (!saved) apply(e.matches ? "dark" : "light");
    });
  })();
</script>
</body>
</html>
```

## Rules for implementation
- Only color tokens change between themes. Layout and spacing stay identical.
- Set `color-scheme` so native controls (scrollbars, inputs) match the theme.
- Never hardcode hex values in components; always reference variables.
- Test every state (hover, focus, error, disabled) in both themes.
