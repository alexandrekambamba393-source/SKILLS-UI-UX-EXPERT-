---
name: brutalism-code-react-tailwind
description: Production-ready React and Tailwind CSS neo-brutalist components with hard offset shadows, thick borders, a press-to-shadow button and oversized headings. Use when building a bold brutalist interface in a React project.
---

# Brutalism: React and Tailwind Implementation

## Tailwind configuration (tailwind.config.js)
```js
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: ["./src/**/*.{js,jsx,ts,tsx}"],
  theme: {
    extend: {
      colors: {
        paper: "#f4f1ea",
        ink: "#000000",
        sun: "#ffd23f",
        alert: "#ff5c5c",
      },
      boxShadow: {
        hard: "6px 6px 0 #000000",
        "hard-lg": "8px 8px 0 #000000",
      },
      fontFamily: {
        display: ["Archivo Black", "Arial Black", "sans-serif"],
        body: ["Inter", "system-ui", "sans-serif"],
      },
    },
  },
  plugins: [],
};
```

## Brutal button (BrutalButton.tsx)
```tsx
import type { ButtonHTMLAttributes } from "react";

export function BrutalButton({ className = "", ...rest }: ButtonHTMLAttributes<HTMLButtonElement>) {
  return (
    <button
      className={[
        "inline-flex min-h-12 items-center border-[3px] border-ink bg-sun px-6",
        "font-extrabold uppercase tracking-wide text-ink shadow-hard",
        "transition-[transform,box-shadow] duration-100 ease-linear",
        "hover:-translate-x-0.5 hover:-translate-y-0.5 hover:shadow-hard-lg",
        "active:translate-x-1.5 active:translate-y-1.5 active:shadow-none",
        "focus-visible:outline-4 focus-visible:outline-offset-4 focus-visible:outline-alert",
        "disabled:cursor-not-allowed disabled:opacity-50 disabled:shadow-none",
        "motion-reduce:transition-none",
        className,
      ].join(" ")}
      {...rest}
    />
  );
}
```

## Card and layout (BrutalLanding.tsx)
```tsx
import { BrutalButton } from "./BrutalButton";

function Card({ title, children }: { title: string; children: React.ReactNode }) {
  return (
    <article className="border-[3px] border-ink bg-white p-6 shadow-hard">
      <h2 className="mb-2 text-2xl font-bold">{title}</h2>
      <p className="m-0">{children}</p>
    </article>
  );
}

export default function BrutalLanding() {
  return (
    <main className="mx-auto grid max-w-5xl gap-10 bg-paper px-6 py-12 font-body text-ink">
      <header className="grid gap-4">
        <span className="w-fit border-2 border-ink bg-alert px-2.5 py-1 text-sm font-bold">Studio 2026</span>
        <h1 className="font-display text-6xl uppercase leading-[0.95] tracking-tight md:text-8xl">
          Build loud. Ship raw.
        </h1>
      </header>

      <div className="grid gap-8 sm:grid-cols-2">
        <Card title="Fast">Pages load in under one second on a standard connection.</Card>
        <Card title="Honest">Every feature is visible. Nothing is hidden behind menus.</Card>
      </div>

      <div>
        <BrutalButton>Start a project</BrutalButton>
      </div>
    </main>
  );
}
```

## Rules for implementation
- Keep radius at zero and shadows unblurred.
- Limit accents to two colours in any single view.
- Make press interactions mechanical, moving the element into its offset.
- Keep body copy in a regular weight; reserve heavy display type for headings.
