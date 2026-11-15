---
name: neo-corporate-code-react-tailwind
description: Production-ready React and Tailwind CSS neo-corporate SaaS components including an app shell, page header with primary action, KPI cards, status badges and a data table. Use when building a professional B2B dashboard or admin console in a React project.
---

# Neo-Corporate: React and Tailwind Implementation

## Tailwind configuration (tailwind.config.js)
```js
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: ["./src/**/*.{js,jsx,ts,tsx}"],
  theme: {
    extend: {
      colors: {
        canvas: "#f7f8fa",
        ink: "#101828",
        body: "#475467",
        line: "#e4e7ec",
        brand: { DEFAULT: "#2f5bea", hover: "#2448c4", soft: "#eef2ff" },
        ok: { DEFAULT: "#067647", bg: "#ecfdf3" },
        warn: { DEFAULT: "#b54708", bg: "#fffaeb" },
      },
      fontFamily: {
        sans: ["Inter", "system-ui", "-apple-system", "Segoe UI", "sans-serif"],
      },
      boxShadow: {
        soft: "0 1px 2px rgba(16, 24, 40, 0.06)",
      },
    },
  },
  plugins: [],
};
```

## Status badge (StatusBadge.tsx)
```tsx
type Status = "healthy" | "degraded";

const styles: Record<Status, { label: string; cls: string }> = {
  healthy: { label: "Healthy", cls: "bg-ok-bg text-ok" },
  degraded: { label: "Degraded", cls: "bg-warn-bg text-warn" },
};

export function StatusBadge({ status }: { status: Status }) {
  const s = styles[status];
  return (
    <span className={`inline-flex items-center gap-1.5 rounded-full px-2.5 py-0.5 text-xs font-semibold ${s.cls}`}>
      {s.label}
    </span>
  );
}
```

## Dashboard (DeploymentsPage.tsx)
```tsx
import { StatusBadge } from "./StatusBadge";

const rows = [
  { service: "payments-api", env: "Production", status: "healthy" as const, updated: "2 min ago" },
  { service: "search-indexer", env: "Production", status: "degraded" as const, updated: "14 min ago" },
];

const kpis = [
  { label: "Active services", value: "42" },
  { label: "Success rate (30d)", value: "99.2%" },
  { label: "Open incidents", value: "1" },
];

export default function DeploymentsPage() {
  return (
    <div className="grid min-h-screen grid-cols-1 bg-canvas font-sans text-ink md:grid-cols-[240px_1fr]">
      <aside className="border-b border-line bg-white p-5 md:border-b-0 md:border-r">
        <div className="px-3 pb-5 text-base font-semibold">Operations</div>
        <nav aria-label="Primary" className="grid gap-1">
          {["Overview", "Deployments", "Billing", "Settings"].map((item) => (
            <a
              key={item}
              href="#"
              aria-current={item === "Overview" ? "page" : undefined}
              className="flex min-h-9 items-center rounded-lg px-3 font-medium text-body hover:bg-slate-100 hover:text-ink aria-[current=page]:bg-brand-soft aria-[current=page]:text-brand focus-visible:outline-2 focus-visible:-outline-offset-2 focus-visible:outline-brand"
            >
              {item}
            </a>
          ))}
        </nav>
      </aside>

      <main className="px-6 py-7 md:px-8">
        <header className="mb-6 flex items-start justify-between gap-4">
          <div>
            <h1 className="mb-1 text-2xl font-semibold">Deployments</h1>
            <p className="text-body">Status of production services, updated 2 minutes ago.</p>
          </div>
          <button className="min-h-10 rounded-lg bg-brand px-4 font-semibold text-white shadow-soft hover:bg-brand-hover focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-brand">
            New deployment
          </button>
        </header>

        <section aria-label="Summary" className="mb-6 grid gap-4 sm:grid-cols-3">
          {kpis.map((k) => (
            <div key={k.label} className="rounded-xl border border-line bg-white p-5 shadow-soft">
              <span className="text-sm text-body">{k.label}</span>
              <strong className="mt-1.5 block text-2xl font-semibold">{k.value}</strong>
            </div>
          ))}
        </section>

        <div className="overflow-x-auto rounded-xl border border-line bg-white shadow-soft">
          <table className="w-full border-collapse">
            <caption className="sr-only">Recent deployments</caption>
            <thead className="bg-[#fafbfc] text-xs font-semibold uppercase tracking-wide text-body">
              <tr>
                <th scope="col" className="px-4 py-3 text-left">Service</th>
                <th scope="col" className="px-4 py-3 text-left">Environment</th>
                <th scope="col" className="px-4 py-3 text-left">Status</th>
                <th scope="col" className="px-4 py-3 text-left">Updated</th>
              </tr>
            </thead>
            <tbody className="text-sm">
              {rows.map((r) => (
                <tr key={r.service} className="border-t border-line">
                  <td className="whitespace-nowrap px-4 py-3">{r.service}</td>
                  <td className="whitespace-nowrap px-4 py-3">{r.env}</td>
                  <td className="whitespace-nowrap px-4 py-3"><StatusBadge status={r.status} /></td>
                  <td className="whitespace-nowrap px-4 py-3">{r.updated}</td>
                </tr>
              ))}
            </tbody>
          </table>
        </div>
      </main>
    </div>
  );
}
```

## Rules for implementation
- Reserve status colours for status; use `brand` for primary actions only.
- Keep the `shadow-soft` and radius values consistent across all cards.
- Pair every badge with text; never rely on colour alone.
- Use `sr-only` for captions that are visually redundant but needed by assistive technology.
