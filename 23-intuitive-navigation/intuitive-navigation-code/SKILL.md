---
name: intuitive-navigation-code
description: Production-ready HTML, CSS and vanilla JavaScript navigation with a primary nav using aria-current, breadcrumbs, a responsive menu toggle that traps nothing but stays keyboard-operable, a search input with live suggestions and a skip link. Use when implementing site or app navigation in plain HTML, CSS and JavaScript.
---

# Intuitive Navigation: HTML, CSS and JavaScript Implementation

```html
<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Navigation Pattern</title>
<style>
  :root { --ink: #111418; --muted: #4a5160; --line: #e4e7ec; --accent: #1f4fd1; --surface: #ffffff; --bg: #f5f6f8; }
  * { box-sizing: border-box; }
  body { margin: 0; font-family: system-ui, sans-serif; color: var(--ink); background: var(--bg); line-height: 1.6; }
  :focus-visible { outline: 3px solid var(--accent); outline-offset: 3px; }

  .skip { position: absolute; left: 12px; top: -60px; background: var(--ink); color: #fff; padding: 10px 14px; border-radius: 8px; }
  .skip:focus { top: 12px; }

  header.site { background: var(--surface); border-bottom: 1px solid var(--line); position: sticky; top: 0; z-index: 10; }
  .bar { display: flex; align-items: center; gap: 16px; padding: 12px 24px; max-width: 1200px; margin: 0 auto; min-height: 64px; }
  .brand { font-weight: 700; color: var(--ink); text-decoration: none; margin-right: auto; }

  .menu-toggle { display: none; min-width: 44px; min-height: 44px; border: 1px solid var(--line); border-radius: 10px; background: #fff; font: inherit; font-weight: 600; cursor: pointer; }

  nav.primary ul { display: flex; gap: 4px; list-style: none; margin: 0; padding: 0; }
  nav.primary a { display: inline-flex; align-items: center; min-height: 44px; padding: 0 12px; color: var(--ink); text-decoration: none; border-radius: 8px; font-weight: 500; }
  nav.primary a:hover { background: var(--bg); }
  nav.primary a[aria-current="page"] { color: var(--accent); background: #eef2ff; font-weight: 600; }

  .search { position: relative; }
  .search input { min-height: 44px; width: 220px; padding: 0 12px; border: 1px solid var(--line); border-radius: 10px; font: inherit; }
  .suggest { position: absolute; top: 48px; left: 0; right: 0; background: #fff; border: 1px solid var(--line); border-radius: 10px; list-style: none; margin: 0; padding: 6px; display: none; box-shadow: 0 10px 24px rgba(0,0,0,0.08); }
  .suggest.open { display: block; }
  .suggest li button { width: 100%; text-align: left; padding: 10px; border: 0; background: none; font: inherit; border-radius: 6px; cursor: pointer; min-height: 40px; }
  .suggest li button:hover, .suggest li button:focus-visible { background: var(--bg); }

  .crumbs { max-width: 1200px; margin: 20px auto 0; padding: 0 24px; font-size: 14px; color: var(--muted); }
  .crumbs ol { display: flex; flex-wrap: wrap; gap: 6px; list-style: none; padding: 0; margin: 0; }
  .crumbs li + li::before { content: "/"; margin-right: 6px; color: #a0a8b6; }
  .crumbs a { color: var(--accent); }

  main { max-width: 1200px; margin: 24px auto; padding: 0 24px 64px; }
  h1 { font-size: 2rem; margin: 0 0 12px; }

  @media (max-width: 860px) {
    .menu-toggle { display: inline-flex; align-items: center; padding: 0 14px; }
    nav.primary { display: none; position: absolute; top: 100%; left: 0; right: 0; background: #fff; border-bottom: 1px solid var(--line); padding: 8px 24px 16px; }
    nav.primary.open { display: block; }
    nav.primary ul { flex-direction: column; }
    nav.primary a { width: 100%; }
    .search input { width: 160px; }
  }
</style>
</head>
<body>
  <a class="skip" href="#main">Skip to content</a>

  <header class="site">
    <div class="bar">
      <a class="brand" href="#">Atlas Docs</a>
      <div class="search" role="search">
        <label for="q" class="sr-only" style="position:absolute;width:1px;height:1px;overflow:hidden;clip:rect(0 0 0 0)">Search documentation</label>
        <input id="q" type="search" placeholder="Search docs" aria-autocomplete="list" aria-controls="suggest" aria-expanded="false">
        <ul id="suggest" class="suggest" role="listbox"></ul>
      </div>
      <button class="menu-toggle" id="toggle" type="button" aria-expanded="false" aria-controls="primary">Menu</button>
      <nav class="primary" id="primary" aria-label="Primary">
        <ul>
          <li><a href="#guides" aria-current="page">Guides</a></li>
          <li><a href="#api">API reference</a></li>
          <li><a href="#changelog">Changelog</a></li>
          <li><a href="#support">Support</a></li>
        </ul>
      </nav>
    </div>
  </header>

  <nav class="crumbs" aria-label="Breadcrumb">
    <ol>
      <li><a href="#">Home</a></li>
      <li><a href="#guides">Guides</a></li>
      <li aria-current="page">Getting started</li>
    </ol>
  </nav>

  <main id="main">
    <h1>Getting started</h1>
    <p>Install the CLI, create a workspace and deploy your first project in under ten minutes.</p>
  </main>

<script>
  const toggle = document.getElementById("toggle");
  const primary = document.getElementById("primary");
  toggle.addEventListener("click", () => {
    const open = primary.classList.toggle("open");
    toggle.setAttribute("aria-expanded", String(open));
    if (open) primary.querySelector("a")?.focus();
  });
  // Close the menu with Escape and return focus to the toggle.
  document.addEventListener("keydown", (e) => {
    if (e.key === "Escape" && primary.classList.contains("open")) {
      primary.classList.remove("open");
      toggle.setAttribute("aria-expanded", "false");
      toggle.focus();
    }
  });

  // Search suggestions
  const pages = ["Getting started", "Authentication", "Deploy a project", "Environment variables", "Billing"];
  const input = document.getElementById("q");
  const list = document.getElementById("suggest");
  input.addEventListener("input", () => {
    const term = input.value.trim().toLowerCase();
    list.innerHTML = "";
    const matches = term ? pages.filter((p) => p.toLowerCase().includes(term)) : [];
    matches.forEach((m) => {
      const li = document.createElement("li");
      const btn = document.createElement("button");
      btn.type = "button";
      btn.textContent = m;
      li.appendChild(btn);
      list.appendChild(li);
    });
    const open = matches.length > 0;
    list.classList.toggle("open", open);
    input.setAttribute("aria-expanded", String(open));
  });
</script>
</body>
</html>
```

## Rules for implementation
- Mark the current page with `aria-current="page"` in the primary navigation and breadcrumbs.
- Keep the menu keyboard-operable; Escape closes it and returns focus to the toggle.
- Keep primary navigation identical across pages and breakpoints.
- Put the skip link first in the DOM.
