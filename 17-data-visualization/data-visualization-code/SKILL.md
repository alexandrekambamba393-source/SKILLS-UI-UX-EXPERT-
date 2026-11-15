---
name: data-visualization-code
description: Production-ready HTML, CSS and vanilla JavaScript data visualisation with an accessible SVG bar chart, a colour-blind-safe palette, a keyboard-reachable tooltip, a KPI tile with trend, a data table fallback and reduced-motion support. Use when implementing accessible charts and KPI tiles in plain HTML, CSS and JavaScript.
---

# Data Visualisation: HTML, CSS and JavaScript Implementation

```html
<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Revenue Dashboard</title>
<style>
  :root {
    --ink: #111418;
    --muted: #5b6470;
    --line: #e4e7ec;
    --surface: #ffffff;
    --bar: #3b5bdb;
    --bar-hover: #2f4bc0;
    --ok: #2b8a3e;
    --warn: #b54708;
    --font: system-ui, sans-serif;
  }
  * { box-sizing: border-box; }
  body { margin: 0; font-family: var(--font); color: var(--ink); background: #f5f6f8; }

  .wrap { max-width: 960px; margin: 40px auto; padding: 0 20px; display: grid; gap: 20px; }

  .kpi { background: var(--surface); border: 1px solid var(--line); border-radius: 14px; padding: 20px 24px; display: flex; justify-content: space-between; align-items: center; }
  .kpi span { color: var(--muted); font-size: 14px; }
  .kpi strong { display: block; font-size: 28px; margin-top: 4px; }
  .kpi .delta { color: var(--ok); font-weight: 600; font-size: 14px; }
  .kpi .delta.down { color: var(--warn); }

  .chart-card { background: var(--surface); border: 1px solid var(--line); border-radius: 14px; padding: 24px; }
  .chart-card h2 { font-size: 1.125rem; margin: 0 0 4px; }
  .chart-card .sub { color: var(--muted); font-size: 14px; margin: 0 0 16px; }

  .chart { width: 100%; height: auto; display: block; }
  .chart .bar { fill: var(--bar); transition: fill 150ms ease, opacity 150ms ease; cursor: pointer; }
  .chart .bar:hover, .chart .bar:focus { fill: var(--bar-hover); outline: none; }
  .chart .bar:focus-visible { stroke: var(--ink); stroke-width: 2; }
  .chart text { font-size: 12px; fill: var(--muted); }

  .tooltip {
    position: absolute; pointer-events: none; background: var(--ink); color: #fff;
    padding: 6px 10px; border-radius: 8px; font-size: 13px; transform: translate(-50%, -120%);
    opacity: 0; transition: opacity 120ms ease;
  }
  .tooltip.show { opacity: 1; }

  table { width: 100%; border-collapse: collapse; font-size: 14px; }
  th, td { text-align: left; padding: 8px 10px; border-bottom: 1px solid var(--line); }
  th { color: var(--muted); font-weight: 600; }
  details summary { cursor: pointer; color: var(--bar); font-weight: 600; margin-top: 12px; }

  @media (prefers-reduced-motion: reduce) { .chart .bar, .tooltip { transition: none; } }
</style>
</head>
<body>
  <main class="wrap">
    <section class="kpi" aria-label="Key figure">
      <div>
        <span>Monthly recurring revenue</span>
        <strong>$248,400</strong>
      </div>
      <div class="delta" aria-label="Up 8.2 percent from last month">+8.2% vs last month</div>
    </section>

    <section class="chart-card" aria-labelledby="chart-title">
      <h2 id="chart-title">Revenue by month (USD thousands)</h2>
      <p class="sub">Revenue grew steadily from July to October, then slowed in November.</p>

      <div style="position:relative">
        <svg id="chart" class="chart" viewBox="0 0 640 300" role="img" aria-labelledby="chart-title chart-desc">
          <desc id="chart-desc">Monthly revenue bars from July to November.</desc>
        </svg>
        <div id="tip" class="tooltip" role="status" aria-live="polite"></div>
      </div>

      <details>
        <summary>View data table</summary>
        <table>
          <thead><tr><th scope="col">Month</th><th scope="col">Revenue (USD k)</th></tr></thead>
          <tbody>
            <tr><td>July</td><td>180</td></tr>
            <tr><td>August</td><td>196</td></tr>
            <tr><td>September</td><td>214</td></tr>
            <tr><td>October</td><td>231</td></tr>
            <tr><td>November</td><td>248</td></tr>
          </tbody>
        </table>
      </details>
    </section>
  </main>

<script>
  const data = [
    { label: "Jul", value: 180 },
    { label: "Aug", value: 196 },
    { label: "Sep", value: 214 },
    { label: "Oct", value: 231 },
    { label: "Nov", value: 248 },
  ];

  const svg = document.getElementById("chart");
  const tip = document.getElementById("tip");
  const W = 640, H = 300, pad = { t: 20, r: 16, b: 36, l: 48 };
  const max = 300;
  const innerW = W - pad.l - pad.r;
  const innerH = H - pad.t - pad.b;
  const barW = innerW / data.length * 0.6;
  const NS = "http://www.w3.org/2000/svg";

  // Gridlines and axis labels start at zero so bar lengths stay honest.
  [0, 100, 200, 300].forEach((v) => {
    const y = pad.t + innerH - (v / max) * innerH;
    const line = document.createElementNS(NS, "line");
    line.setAttribute("x1", pad.l); line.setAttribute("x2", W - pad.r);
    line.setAttribute("y1", y); line.setAttribute("y2", y);
    line.setAttribute("stroke", "#e4e7ec");
    svg.appendChild(line);
    const t = document.createElementNS(NS, "text");
    t.setAttribute("x", pad.l - 8); t.setAttribute("y", y + 4); t.setAttribute("text-anchor", "end");
    t.textContent = v;
    svg.appendChild(t);
  });

  data.forEach((d, i) => {
    const x = pad.l + (innerW / data.length) * i + (innerW / data.length - barW) / 2;
    const h = (d.value / max) * innerH;
    const y = pad.t + innerH - h;

    const rect = document.createElementNS(NS, "rect");
    rect.setAttribute("class", "bar");
    rect.setAttribute("x", x); rect.setAttribute("y", y);
    rect.setAttribute("width", barW); rect.setAttribute("height", h);
    rect.setAttribute("rx", 6);
    rect.setAttribute("tabindex", "0");
    rect.setAttribute("role", "img");
    rect.setAttribute("aria-label", `${d.label}: ${d.value} thousand dollars`);

    const show = () => {
      const box = rect.getBoundingClientRect();
      const parent = svg.getBoundingClientRect();
      tip.textContent = `${d.label}: $${d.value}k`;
      tip.style.left = `${box.left - parent.left + box.width / 2}px`;
      tip.style.top = `${box.top - parent.top}px`;
      tip.classList.add("show");
    };
    const hide = () => tip.classList.remove("show");
    rect.addEventListener("mouseenter", show);
    rect.addEventListener("focus", show);
    rect.addEventListener("mouseleave", hide);
    rect.addEventListener("blur", hide);
    svg.appendChild(rect);

    const label = document.createElementNS(NS, "text");
    label.setAttribute("x", x + barW / 2); label.setAttribute("y", H - 12); label.setAttribute("text-anchor", "middle");
    label.textContent = d.label;
    svg.appendChild(label);
  });
</script>
</body>
</html>
```

## Rules for implementation
- Start bar axes at zero and label gridlines with values.
- Make every bar keyboard-focusable with an `aria-label` carrying the value.
- Provide a data table as an alternative beside the chart.
- Pair colour with labels; do not rely on hue alone to distinguish values.
