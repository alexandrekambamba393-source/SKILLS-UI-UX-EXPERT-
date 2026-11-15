---
name: empty-states-code
description: Production-ready HTML and CSS empty states for first use, no search results and error with retry, including a filter-aware results status, a clear primary action and accessible headings. Use when implementing empty, no-results and error screens in plain HTML and CSS.
---

# Empty States: HTML and CSS Implementation

```html
<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Empty States</title>
<style>
  :root { --ink: #111418; --muted: #4a5160; --line: #c9d0db; --accent: #1f4fd1; --surface: #fff; --bg: #f5f6f8; --danger: #b42318; }
  * { box-sizing: border-box; }
  body { margin: 0; font-family: system-ui, sans-serif; color: var(--ink); background: var(--bg); line-height: 1.55; }
  :focus-visible { outline: 3px solid var(--accent); outline-offset: 3px; }

  .page { max-width: 880px; margin: 40px auto; padding: 0 20px; display: grid; gap: 28px; }
  .section-title { font-size: 1.1rem; margin: 0; color: var(--muted); font-weight: 600; text-transform: uppercase; letter-spacing: 0.04em; }

  .empty {
    background: var(--surface);
    border: 1.5px dashed var(--line);
    border-radius: 16px;
    padding: 40px 24px;
    text-align: center;
    display: grid;
    justify-items: center;
    gap: 10px;
  }
  .empty .icon { width: 56px; height: 56px; border-radius: 16px; background: #eef2ff; display: grid; place-items: center; color: var(--accent); margin-bottom: 6px; }
  .empty h3 { margin: 0; font-size: 1.25rem; }
  .empty p { margin: 0; max-width: 46ch; color: var(--muted); }
  .actions { display: flex; flex-wrap: wrap; gap: 12px; justify-content: center; margin-top: 8px; }

  .btn { min-height: 44px; padding: 0 18px; border-radius: 10px; font: inherit; font-weight: 600; cursor: pointer; border: 1.5px solid transparent; text-decoration: none; display: inline-flex; align-items: center; }
  .btn-primary { background: var(--accent); color: #fff; }
  .btn-ghost { background: #fff; color: var(--ink); border-color: var(--line); }

  .search-row { display: flex; gap: 10px; flex-wrap: wrap; }
  .search-row input { flex: 1 1 240px; min-height: 44px; padding: 0 12px; font: inherit; border: 1.5px solid var(--line); border-radius: 10px; }
  .status { color: var(--muted); font-size: 14px; margin: 0; }

  .error { border-color: #f1b8b3; background: #fffbfa; }
  .error .icon { background: #fef3f2; color: var(--danger); }
</style>
</head>
<body>
  <main class="page">
    <section aria-labelledby="first-use-title">
      <h2 class="section-title">First use</h2>
      <div class="empty">
        <div class="icon" aria-hidden="true">
          <svg width="26" height="26" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M12 5v14M5 12h14"/></svg>
        </div>
        <h3 id="first-use-title">No projects yet</h3>
        <p>Projects hold your files, tasks and team activity. Create one to see your dashboard come to life.</p>
        <div class="actions">
          <button class="btn btn-primary" type="button">Create a project</button>
          <a class="btn btn-ghost" href="#templates">Browse templates</a>
        </div>
      </div>
    </section>

    <section aria-labelledby="search-title">
      <h2 class="section-title" id="search-title">Search results</h2>
      <div class="search-row">
        <label for="q" class="sr-only" style="position:absolute;width:1px;height:1px;overflow:hidden;clip:rect(0 0 0 0)">Search projects</label>
        <input id="q" type="search" value="budget 2031" aria-describedby="results-status">
        <button class="btn btn-ghost" type="button" id="clear">Clear filters</button>
      </div>
      <p id="results-status" class="status" role="status" aria-live="polite" style="margin-top:12px">0 results for "budget 2031"</p>
      <div class="empty" style="margin-top:16px">
        <h3>No results for "budget 2031"</h3>
        <p>Check the spelling, try a shorter term, or remove the year from your search.</p>
        <div class="actions">
          <button class="btn btn-primary" type="button" id="reset-search">Clear search</button>
        </div>
      </div>
    </section>

    <section aria-labelledby="error-title">
      <h2 class="section-title" id="error-title">Error</h2>
      <div class="empty error" role="alert">
        <div class="icon" aria-hidden="true">
          <svg width="26" height="26" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><circle cx="12" cy="12" r="9"/><path d="M12 8v5M12 16h.01"/></svg>
        </div>
        <h3>We could not load your reports</h3>
        <p>This is usually temporary. Try again in a moment, or check your connection.</p>
        <div class="actions">
          <button class="btn btn-primary" type="button">Try again</button>
          <a class="btn btn-ghost" href="#status">View service status</a>
        </div>
      </div>
    </section>
  </main>

<script>
  const q = document.getElementById("q");
  const status = document.getElementById("results-status");
  document.getElementById("reset-search").addEventListener("click", () => {
    q.value = "";
    q.focus();
    status.textContent = "Showing all projects";
  });
  document.getElementById("clear").addEventListener("click", () => {
    q.value = "";
    status.textContent = "Showing all projects";
  });
</script>
</body>
</html>
```

## Rules for implementation
- Give each empty state a real heading and one primary action.
- Use `role="alert"` only for errors that need immediate attention; use `role="status"` for result counts.
- Explain why the state is empty and what to do next, in one or two sentences.
- Keep illustrations decorative (`aria-hidden`) and never rely on them alone.
