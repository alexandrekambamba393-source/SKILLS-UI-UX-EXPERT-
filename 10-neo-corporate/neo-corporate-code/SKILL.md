---
name: neo-corporate-code
description: Production-ready HTML and CSS neo-corporate SaaS layout with a sidebar app shell, page header with primary action, a KPI card row, a data table with status badges and accessible focus states. Use when implementing a professional B2B dashboard or admin screen in plain HTML and CSS.
---

# Neo-Corporate: HTML and CSS Implementation

```html
<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Operations Console</title>
<style>
  :root {
    --bg: #f7f8fa;
    --surface: #ffffff;
    --ink: #101828;
    --muted: #475467;
    --line: #e4e7ec;
    --accent: #2f5bea;
    --accent-hover: #2448c4;
    --ok: #067647;
    --ok-bg: #ecfdf3;
    --warn: #b54708;
    --warn-bg: #fffaeb;
    --radius-ctl: 8px;
    --radius-card: 12px;
    --shadow: 0 1px 2px rgba(16, 24, 40, 0.06);
    --font: "Inter", system-ui, -apple-system, "Segoe UI", sans-serif;
  }

  * { box-sizing: border-box; }
  body { margin: 0; font-family: var(--font); font-size: 14px; color: var(--ink); background: var(--bg); display: grid; grid-template-columns: 240px 1fr; min-height: 100vh; }

  .sidebar { background: var(--surface); border-right: 1px solid var(--line); padding: 20px 12px; }
  .sidebar .brand { font-weight: 600; padding: 8px 12px 20px; font-size: 16px; }
  .sidebar nav a {
    display: flex; align-items: center; min-height: 36px; padding: 0 12px;
    border-radius: var(--radius-ctl); color: var(--muted); text-decoration: none; font-weight: 500;
  }
  .sidebar nav a[aria-current="page"] { background: #eef2ff; color: var(--accent); }
  .sidebar nav a:hover { background: #f2f4f7; color: var(--ink); }
  .sidebar nav a:focus-visible { outline: 2px solid var(--accent); outline-offset: -2px; }

  main { padding: 28px 32px 48px; }

  .page-header { display: flex; justify-content: space-between; align-items: flex-start; gap: 16px; margin-bottom: 24px; }
  .page-header h1 { margin: 0 0 4px; font-size: 24px; font-weight: 600; }
  .page-header p { margin: 0; color: var(--muted); }

  .btn {
    min-height: 40px; padding: 0 16px; border: 1px solid transparent; border-radius: var(--radius-ctl);
    font: inherit; font-weight: 600; cursor: pointer; background: var(--accent); color: #fff; box-shadow: var(--shadow);
  }
  .btn:hover { background: var(--accent-hover); }
  .btn:focus-visible { outline: 2px solid var(--accent); outline-offset: 2px; }

  .kpis { display: grid; grid-template-columns: repeat(3, minmax(0, 1fr)); gap: 16px; margin-bottom: 24px; }
  .kpi { background: var(--surface); border: 1px solid var(--line); border-radius: var(--radius-card); padding: 16px 20px; box-shadow: var(--shadow); }
  .kpi span { color: var(--muted); font-size: 13px; }
  .kpi strong { display: block; margin-top: 6px; font-size: 24px; font-weight: 600; }

  .table-wrap { background: var(--surface); border: 1px solid var(--line); border-radius: var(--radius-card); overflow-x: auto; box-shadow: var(--shadow); }
  table { width: 100%; border-collapse: collapse; }
  th, td { text-align: left; padding: 12px 16px; border-bottom: 1px solid var(--line); white-space: nowrap; }
  th { font-size: 12px; font-weight: 600; text-transform: uppercase; letter-spacing: 0.04em; color: var(--muted); background: #fafbfc; }
  tr:last-child td { border-bottom: 0; }

  .badge { display: inline-flex; align-items: center; gap: 6px; padding: 2px 10px; border-radius: 999px; font-size: 12px; font-weight: 600; }
  .badge.ok { background: var(--ok-bg); color: var(--ok); }
  .badge.warn { background: var(--warn-bg); color: var(--warn); }

  @media (max-width: 900px) {
    body { grid-template-columns: 1fr; }
    .sidebar { border-right: 0; border-bottom: 1px solid var(--line); }
    .kpis { grid-template-columns: 1fr; }
  }
</style>
</head>
<body>
  <aside class="sidebar">
    <div class="brand">Operations</div>
    <nav aria-label="Primary">
      <a href="#" aria-current="page">Overview</a>
      <a href="#">Deployments</a>
      <a href="#">Billing</a>
      <a href="#">Settings</a>
    </nav>
  </aside>

  <main>
    <header class="page-header">
      <div>
        <h1>Deployments</h1>
        <p>Status of production services, updated 2 minutes ago.</p>
      </div>
      <button class="btn">New deployment</button>
    </header>

    <section class="kpis" aria-label="Summary">
      <div class="kpi"><span>Active services</span><strong>42</strong></div>
      <div class="kpi"><span>Success rate (30d)</span><strong>99.2%</strong></div>
      <div class="kpi"><span>Open incidents</span><strong>1</strong></div>
    </section>

    <div class="table-wrap">
      <table>
        <caption style="position:absolute;width:1px;height:1px;overflow:hidden;clip:rect(0 0 0 0)">Recent deployments</caption>
        <thead>
          <tr><th scope="col">Service</th><th scope="col">Environment</th><th scope="col">Status</th><th scope="col">Updated</th></tr>
        </thead>
        <tbody>
          <tr><td>payments-api</td><td>Production</td><td><span class="badge ok">Healthy</span></td><td>2 min ago</td></tr>
          <tr><td>search-indexer</td><td>Production</td><td><span class="badge warn">Degraded</span></td><td>14 min ago</td></tr>
        </tbody>
      </table>
    </div>
  </main>
</body>
</html>
```

## Rules for implementation
- Keep one accent colour and one neutral palette; status colours are reserved for status.
- Keep radius and shadow values as variables and reuse them across all components.
- Use `scope` on table headers and a `caption` (visually hidden if redundant) for screen readers.
- Pair every status badge with text, never colour alone.
