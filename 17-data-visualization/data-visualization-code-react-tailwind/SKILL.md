---
name: data-visualization-code-react-tailwind
description: Production-ready React and Tailwind CSS data visualisation with a typed accessible bar chart, a KPI card with trend, a palette helper that avoids colour-only encoding and a data table fallback. Use when building accessible charts and KPI widgets in a React project.
---

# Data Visualisation: React and Tailwind Implementation

## Palette and types (chartTheme.ts)
```ts
// Okabe-Ito inspired categorical palette, safe for common colour-vision deficiencies.
export const palette = ["#0072B2", "#E69F00", "#009E73", "#D55E00", "#CC79A7", "#56B4E9", "#F0E442"] as const;

export interface Point {
  label: string;
  value: number;
}

export function formatCurrencyK(n: number) {
  return `$${n.toLocaleString("en-US")}k`;
}
```

## Accessible bar chart (BarChart.tsx)
```tsx
import { useState } from "react";
import { formatCurrencyK, type Point } from "./chartTheme";

export function BarChart({ id, title, data, max }: { id: string; title: string; data: Point[]; max: number }) {
  const [active, setActive] = useState<number | null>(null);
  const W = 640;
  const H = 300;
  const pad = { t: 20, r: 16, b: 36, l: 48 };
  const innerW = W - pad.l - pad.r;
  const innerH = H - pad.t - pad.b;
  const slot = innerW / data.length;
  const barW = slot * 0.6;
  const ticks = [0, max / 3, (2 * max) / 3, max].map((v) => Math.round(v));

  return (
    <figure className="rounded-2xl border border-slate-200 bg-white p-6">
      <figcaption id={`${id}-title`} className="mb-1 text-lg font-semibold">{title}</figcaption>

      <div className="relative">
        <svg viewBox={`0 0 ${W} ${H}`} role="img" aria-labelledby={`${id}-title`} className="block h-auto w-full">
          {ticks.map((t) => {
            const y = pad.t + innerH - (t / max) * innerH;
            return (
              <g key={t}>
                <line x1={pad.l} x2={W - pad.r} y1={y} y2={y} stroke="#e4e7ec" />
                <text x={pad.l - 8} y={y + 4} textAnchor="end" className="fill-slate-500 text-[12px]">{t}</text>
              </g>
            );
          })}

          {data.map((d, i) => {
            const h = (d.value / max) * innerH;
            const x = pad.l + slot * i + (slot - barW) / 2;
            const y = pad.t + innerH - h;
            const isActive = active === i;
            return (
              <g key={d.label}>
                <rect
                  x={x}
                  y={y}
                  width={barW}
                  height={h}
                  rx={6}
                  tabIndex={0}
                  aria-label={`${d.label}: ${formatCurrencyK(d.value)}`}
                  onMouseEnter={() => setActive(i)}
                  onMouseLeave={() => setActive(null)}
                  onFocus={() => setActive(i)}
                  onBlur={() => setActive(null)}
                  className={`cursor-pointer transition-[fill,stroke] duration-150 focus:outline-none focus-visible:stroke-ink focus-visible:stroke-2 motion-reduce:transition-none ${
                    isActive ? "fill-[#2f4bc0]" : "fill-brand"
                  }`}
                />
                <text x={x + barW / 2} y={H - 12} textAnchor="middle" className="fill-slate-500 text-[12px]">
                  {d.label}
                </text>
                {isActive && (
                  <text x={x + barW / 2} y={y - 8} textAnchor="middle" className="fill-ink text-[13px] font-semibold">
                    {formatCurrencyK(d.value)}
                  </text>
                )}
              </g>
            );
          })}
        </svg>
      </div>

      <details className="mt-4">
        <summary className="cursor-pointer font-semibold text-brand">View data table</summary>
        <table className="mt-3 w-full border-collapse text-sm">
          <thead>
            <tr className="text-left text-slate-500">
              <th scope="col" className="border-b border-slate-200 py-2">Month</th>
              <th scope="col" className="border-b border-slate-200 py-2">Value</th>
            </tr>
          </thead>
          <tbody>
            {data.map((d) => (
              <tr key={d.label}>
                <td className="border-b border-slate-100 py-2">{d.label}</td>
                <td className="border-b border-slate-100 py-2">{formatCurrencyK(d.value)}</td>
              </tr>
            ))}
          </tbody>
        </table>
      </details>
    </figure>
  );
}
```

## KPI card (KpiCard.tsx)
```tsx
export function KpiCard({ label, value, delta }: { label: string; value: string; delta: number }) {
  const positive = delta >= 0;
  return (
    <div className="flex items-center justify-between rounded-2xl border border-slate-200 bg-white px-6 py-5">
      <div>
        <p className="text-sm text-slate-500">{label}</p>
        <p className="mt-1 text-3xl font-bold tracking-tight">{value}</p>
      </div>
      <p
        className={`text-sm font-semibold ${positive ? "text-ok" : "text-danger"}`}
        aria-label={`${positive ? "Up" : "Down"} ${Math.abs(delta)} percent from last month`}
      >
        {positive ? "+" : ""}
        {delta}% vs last month
      </p>
    </div>
  );
}
```

## Usage (RevenueDashboard.tsx)
```tsx
import { BarChart } from "./BarChart";
import { KpiCard } from "./KpiCard";

const revenue = [
  { label: "Jul", value: 180 },
  { label: "Aug", value: 196 },
  { label: "Sep", value: 214 },
  { label: "Oct", value: 231 },
  { label: "Nov", value: 248 },
];

export default function RevenueDashboard() {
  return (
    <main className="mx-auto grid max-w-4xl gap-5 bg-slate-50 p-6 font-sans text-ink">
      <KpiCard label="Monthly recurring revenue" value="$248,400" delta={8.2} />
      <BarChart id="revenue" title="Revenue by month (USD thousands)" data={revenue} max={300} />
    </main>
  );
}
```

## Rules for implementation
- Start bars at zero and label ticks with values.
- Use a colour-blind-aware palette and never encode meaning in colour alone.
- Make each bar focusable and announce its value through `aria-label`.
- Always include a data table alternative.
