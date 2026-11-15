---
name: responsive-design-code-react-tailwind
description: Production-ready React and Tailwind CSS responsive layout with a mobile-first header and menu toggle, fluid typography, an auto-fit card grid and touch-friendly targets. Use when building a responsive page or app shell in a React project.
---

# Responsive Design: React and Tailwind Implementation

## Tailwind configuration (tailwind.config.js)
```js
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: ["./src/**/*.{js,jsx,ts,tsx}"],
  theme: {
    screens: {
      sm: "480px",
      md: "768px",
      lg: "1024px",
      xl: "1280px",
    },
    extend: {
      fontSize: {
        fluid: ["clamp(1rem, 0.95rem + 0.25vw, 1.125rem)", { lineHeight: "1.6" }],
        "fluid-h1": ["clamp(2rem, 1.4rem + 2.5vw, 3.25rem)", { lineHeight: "1.1", letterSpacing: "-0.02em" }],
      },
      colors: {
        ink: "#111418",
        muted: "#4a5160",
        line: "#e4e7ec",
        brand: "#3b5bdb",
        canvas: "#f5f6f8",
      },
    },
  },
  plugins: [],
};
```

## Responsive header (SiteHeader.tsx)
```tsx
import { useState } from "react";

const links = [
  { href: "#products", label: "Products" },
  { href: "#pricing", label: "Pricing" },
  { href: "#support", label: "Support" },
];

export function SiteHeader() {
  const [open, setOpen] = useState(false);

  return (
    <header className="sticky top-0 z-10 border-b border-line bg-white">
      <div className="flex min-h-16 items-center justify-between gap-4 px-4 py-3 sm:px-10">
        <a href="#" className="font-bold text-ink no-underline">Northwind</a>
        <button
          type="button"
          aria-expanded={open}
          aria-controls="primary-nav"
          onClick={() => setOpen((v) => !v)}
          className="min-h-11 min-w-11 rounded-lg border border-line bg-white px-3 font-semibold md:hidden focus-visible:outline-[3px] focus-visible:outline-offset-3 focus-visible:outline-brand"
        >
          Menu
        </button>
      </div>

      <nav
        id="primary-nav"
        aria-label="Primary"
        className={`${open ? "grid" : "hidden"} gap-1 border-t border-line px-4 pb-4 pt-2 sm:px-10 md:flex md:gap-2 md:border-0 md:p-0`}
      >
        {links.map((l) => (
          <a
            key={l.href}
            href={l.href}
            className="flex min-h-11 items-center rounded-lg px-2 text-ink no-underline hover:bg-canvas md:min-h-10"
          >
            {l.label}
          </a>
        ))}
      </nav>
    </header>
  );
}
```

## Page layout (ResponsivePage.tsx)
```tsx
import { SiteHeader } from "./SiteHeader";

const features = [
  { title: "Fast setup", body: "Get your team running in minutes, from any device." },
  { title: "Shared views", body: "Dashboards keep the same information on every screen size." },
  { title: "Offline access", body: "Work continues when the connection is unstable." },
];

export default function ResponsivePage() {
  return (
    <div className="min-h-screen bg-canvas font-sans text-fluid text-ink">
      <a href="#main" className="sr-only focus:not-sr-only focus:absolute focus:left-3 focus:top-3 focus:rounded-lg focus:bg-ink focus:px-3 focus:py-2 focus:text-white">
        Skip to content
      </a>
      <SiteHeader />

      <main id="main" className="mx-auto max-w-6xl px-4 pb-16 pt-8 sm:px-10">
        <h1 className="mb-4 max-w-[22ch] text-fluid-h1 font-bold">Software that adapts to every screen</h1>
        <p className="mb-8 max-w-[60ch] text-muted">
          Content reflows by priority. The primary action stays reachable on phones, tablets and large displays alike.
        </p>

        <section aria-label="Features" className="grid gap-5 [grid-template-columns:repeat(auto-fit,minmax(min(100%,260px),1fr))]">
          {features.map((f) => (
            <article key={f.title} className="rounded-2xl border border-line bg-white p-6">
              <h2 className="mb-2 text-xl font-semibold">{f.title}</h2>
              <p className="text-muted">{f.body}</p>
            </article>
          ))}
        </section>

        <a
          href="#start"
          className="mt-8 inline-flex min-h-12 items-center justify-center rounded-xl bg-brand px-6 font-semibold text-white no-underline focus-visible:outline-[3px] focus-visible:outline-offset-3 focus-visible:outline-brand"
        >
          Start free trial
        </a>
      </main>
    </div>
  );
}
```

## Rules for implementation
- Start with base styles for the smallest screen and extend with `sm:`, `md:` and `lg:` upward.
- Use the fluid font utilities and `[grid-template-columns:...]` arbitrary values only where a standard utility does not exist.
- Keep touch targets at 44px minimum and collapse navigation into a menu below `md`.
- Keep the content hierarchy identical across breakpoints; only reflow the layout.
