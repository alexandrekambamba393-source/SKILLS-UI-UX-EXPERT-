---
name: swiss-design-code-react-tailwind
description: Production-ready React and Tailwind CSS Swiss-style components using a 12-column grid, a modular type scale, flush-left text and a single accent colour. Use when building a grid-driven, typographic interface in a React project.
---

# Swiss Design: React and Tailwind Implementation

## Tailwind configuration (tailwind.config.js)
```js
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: ["./src/**/*.{js,jsx,ts,tsx}"],
  theme: {
    extend: {
      colors: {
        paper: "#ffffff",
        ink: "#0a0a0a",
        grey: "#6e6e6e",
        rule: "#d8d8d8",
        signal: "#e10600",
      },
      fontFamily: {
        sans: ["Helvetica Neue", "Helvetica", "Arial", "Inter", "sans-serif"],
      },
      fontSize: {
        // Modular scale, ratio 1.25
        base: ["1rem", { lineHeight: "1.5" }],
        lg: ["1.25rem", { lineHeight: "1.5" }],
        xl: ["1.5625rem", { lineHeight: "1.35" }],
        "2xl": ["1.953rem", { lineHeight: "1.2" }],
        display: ["clamp(3rem, 7vw, 5.25rem)", { lineHeight: "1", letterSpacing: "-0.03em" }],
      },
      gridTemplateColumns: {
        swiss: "repeat(12, minmax(0, 1fr))",
      },
      columnGap: {
        swiss: "24px",
      },
    },
  },
  plugins: [],
};
```

## Grid components (SwissLayout.tsx)
```tsx
import type { ReactNode } from "react";

export function SwissGrid({ children }: { children: ReactNode }) {
  return (
    <div className="mx-auto grid max-w-[1280px] grid-cols-swiss gap-x-[24px] px-6 py-16 sm:px-9">
      {children}
    </div>
  );
}

export function Headline({ children }: { children: ReactNode }) {
  return (
    <h1 className="col-span-full mb-12 text-display font-bold text-ink lg:col-span-8">
      {children}
    </h1>
  );
}

export function Intro({ children }: { children: ReactNode }) {
  return <p className="col-span-full text-lg leading-relaxed text-ink lg:col-span-4 lg:col-start-9">{children}</p>;
}

export function Rule() {
  return <hr className="col-span-full my-12 border-t border-ink" />;
}

export function Label({ children }: { children: ReactNode }) {
  return (
    <p className="col-span-full text-sm font-semibold uppercase tracking-wide text-grey lg:col-span-3">
      {children}
    </p>
  );
}

export function Body({ children }: { children: ReactNode }) {
  return <p className="col-span-full max-w-prose text-base text-ink lg:col-span-6 lg:col-start-4">{children}</p>;
}

export function Figure({ children }: { children: ReactNode }) {
  return <p className="col-span-full text-2xl font-bold text-signal lg:col-span-3 lg:col-start-10">{children}</p>;
}
```

## Page (SwissPage.tsx)
```tsx
import { Body, Figure, Headline, Intro, Label, Rule, SwissGrid } from "./SwissLayout";

export default function SwissPage() {
  return (
    <main className="bg-paper font-sans">
      <SwissGrid>
        <Headline>Order through the grid.</Headline>
        <Intro>Every element sits on a visible system. Hierarchy comes from scale and position, not ornament.</Intro>
        <Rule />
        <Label>Method</Label>
        <Body>Set text flush left and ragged right. Align each block to column edges and build spacing on a single baseline.</Body>
        <Figure>12</Figure>
        <Rule />
        <a
          href="#"
          className="col-span-full inline-flex min-h-11 w-fit items-center bg-ink px-4 font-semibold text-paper hover:bg-signal focus-visible:outline-2 focus-visible:outline-offset-4 focus-visible:outline-signal lg:col-span-3"
        >
          Read the system
        </a>
      </SwissGrid>
    </main>
  );
}
```

## Rules for implementation
- Place every block with grid column utilities, not arbitrary margins.
- Keep text flush left; avoid `text-center` on body copy.
- Use a single signal colour and reserve it for emphasis.
- Keep body line length within `max-w-prose`.
