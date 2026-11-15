---
name: bento-grid-code-react-tailwind
description: Production-ready React and Tailwind CSS bento grid with typed tile spans, a hero tile, responsive reflow and full-tile links. Use when building a modular card-based feature or dashboard layout in a React project.
---

# Bento Grid: React and Tailwind Implementation

## Tile component (BentoTile.tsx)
```tsx
import type { ReactNode } from "react";

export type Span = "1x1" | "2x1" | "1x2" | "2x2";

const spanClasses: Record<Span, string> = {
  "1x1": "",
  "2x1": "md:col-span-2",
  "1x2": "md:row-span-2",
  "2x2": "md:col-span-2 md:row-span-2",
};

interface BentoTileProps {
  span?: Span;
  href?: string;
  title: string;
  hero?: boolean;
  children?: ReactNode;
}

export function BentoTile({ span = "1x1", href, title, hero = false, children }: BentoTileProps) {
  const base =
    "group relative flex flex-col justify-between overflow-hidden rounded-3xl p-6 " +
    "transition-[transform,box-shadow] duration-200 ease-out motion-reduce:transition-none " +
    "hover:-translate-y-0.5 hover:shadow-lg";
  const tone = hero
    ? "bg-gradient-to-br from-[#3b5bdb] to-[#7048e8] text-white"
    : "bg-white text-ink ring-1 ring-black/5";

  return (
    <article className={`${base} ${tone} ${spanClasses[span]}`}>
      <div>
        <h2 className="mb-2 text-xl font-semibold">{title}</h2>
        <div className={hero ? "text-white/85" : "text-muted"}>{children}</div>
      </div>
      {href && (
        <a
          href={href}
          aria-label={`Open ${title}`}
          className="absolute inset-0 rounded-3xl focus-visible:outline-[3px] focus-visible:outline-offset-2 focus-visible:outline-accent"
        />
      )}
    </article>
  );
}
```

## Grid (BentoGrid.tsx)
```tsx
import { BentoTile } from "./BentoTile";

export default function BentoGrid() {
  return (
    <main className="mx-auto my-12 grid max-w-5xl auto-rows-[200px] grid-cols-1 gap-5 px-6 md:grid-cols-4">
      <BentoTile span="2x2" hero title="Unified analytics" href="#analytics">
        <p>Every key metric in one view, updated in real time across your teams.</p>
        <p className="mt-6 text-5xl font-bold tracking-tight text-white">98.4%</p>
      </BentoTile>

      <BentoTile title="Alerts" href="#alerts">
        <p>Get notified only when it matters.</p>
      </BentoTile>

      <BentoTile span="2x1" title="Team activity" href="#activity">
        <p>See who changed what, and when, without digging through logs.</p>
      </BentoTile>

      <BentoTile title="Exports" href="#exports">
        <p>CSV, PDF and API access.</p>
      </BentoTile>
    </main>
  );
}
```

## Rules for implementation
- Keep the span mapping in one place (`spanClasses`) so the grid is changed from data, not scattered classes.
- Use `md:` breakpoints so tiles collapse to a single column on mobile.
- Keep the full-tile link as an absolutely positioned overlay with an accessible label.
- Keep DOM order equal to priority order.
