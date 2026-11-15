---
name: page-transitions-code
description: Production-ready HTML, CSS and vanilla JavaScript page transitions using the View Transitions API with a cross-fade fallback, shared-element naming, focus management after navigation and reduced-motion support. Use when implementing smooth transitions between views in plain HTML, CSS and JavaScript.
---

# Page Transitions: HTML, CSS and JavaScript Implementation

```html
<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>View Transitions</title>
<style>
  :root { --ink: #111418; --muted: #5b6470; --accent: #3b5bdb; --paper: #ffffff; --line: #e4e7ec; }
  * { box-sizing: border-box; }
  body { margin: 0; font-family: system-ui, sans-serif; color: var(--ink); background: var(--paper); }

  .topbar { display: flex; gap: 16px; padding: 16px 24px; border-bottom: 1px solid var(--line); position: sticky; top: 0; background: var(--paper); z-index: 2; }
  .topbar a { color: var(--accent); font-weight: 600; }
  .topbar a:focus-visible { outline: 2px solid var(--accent); outline-offset: 2px; }

  .view { max-width: 720px; margin: 48px auto; padding: 0 24px; }
  .view h1 { font-size: clamp(2rem, 5vw, 2.75rem); letter-spacing: -0.02em; margin: 0 0 16px; }
  .view p { color: var(--muted); line-height: 1.7; }

  .thumb { width: 96px; height: 96px; border-radius: 16px; object-fit: cover; background: #e6e8ec; view-transition-name: hero-image; }
  .detail-hero { width: 100%; height: 280px; border-radius: 20px; object-fit: cover; background: #e6e8ec; view-transition-name: hero-image; }

  .card-link { display: flex; gap: 16px; align-items: center; padding: 16px; border: 1px solid var(--line); border-radius: 16px; text-decoration: none; color: inherit; margin-bottom: 16px; }
  .card-link:hover { border-color: #c7cdd6; }
  .card-link:focus-visible { outline: 2px solid var(--accent); outline-offset: 2px; }

  .back { display: inline-block; margin-bottom: 24px; color: var(--accent); font-weight: 600; }

  /* Fallback cross-fade is provided by the browser default; customise timing here */
  ::view-transition-old(root), ::view-transition-new(root) { animation-duration: 260ms; }
  @media (prefers-reduced-motion: reduce) {
    ::view-transition-old(*), ::view-transition-new(*) { animation: none !important; }
  }
</style>
</head>
<body>
  <nav class="topbar" aria-label="Main">
    <a href="#list">Projects</a>
  </nav>

  <main id="app" class="view" tabindex="-1"></main>

<script>
  const app = document.getElementById("app");

  const projects = [
    { id: "atlas", title: "Atlas", summary: "A mapping tool for field teams that works offline.", img: "https://images.unsplash.com/photo-1507525428034-b723cf961d3e?w=400" },
    { id: "beacon", title: "Beacon", summary: "Real-time safety alerts for distributed workplaces.", img: "https://images.unsplash.com/photo-1518770660439-4636190af475?w=400" },
  ];

  const views = {
    list() {
      return `
        <h1>Projects</h1>
        ${projects.map((p) => `
          <a class="card-link" href="#project/${p.id}">
            <img class="thumb" src="${p.img}" alt="" style="view-transition-name: thumb-${p.id}">
            <div><strong>${p.title}</strong><p style="margin:4px 0 0">${p.summary}</p></div>
          </a>`).join("")}`;
    },
    project(id) {
      const p = projects.find((x) => x.id === id);
      if (!p) return `<h1>Not found</h1><p>This project does not exist.</p>`;
      return `
        <a class="back" href="#list">Back to projects</a>
        <img class="detail-hero" src="${p.img}" alt="" style="view-transition-name: thumb-${p.id}">
        <h1>${p.title}</h1>
        <p>${p.summary}</p>`;
    },
  };

  function render() {
    const hash = location.hash.replace(/^#/, "") || "list";
    const [route, id] = hash.split("/");
    const html = (views[route] ? views[route](id) : views.list());
    app.innerHTML = html;
    const heading = app.querySelector("h1");
    if (heading) heading.setAttribute("tabindex", "-1");
    (heading || app).focus({ preventScroll: true });
  }

  function navigate() {
    if (!document.startViewTransition || window.matchMedia("(prefers-reduced-motion: reduce)").matches) {
      render();
      return;
    }
    document.startViewTransition(() => render());
  }

  window.addEventListener("hashchange", navigate);
  render();
</script>
</body>
</html>
```

## Rules for implementation
- Use `document.startViewTransition` when available; fall back to an immediate render otherwise.
- Name shared elements with `view-transition-name`, and keep each name unique on the page.
- Move focus to the new page heading after every navigation.
- Skip animation when `prefers-reduced-motion` is set, as shown.
