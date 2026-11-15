---
name: gradient-design-code-react-tailwind
description: Production-ready React and Tailwind CSS gradient components including a mesh hero, a gradient button, a gradient text heading and a reduced-motion-aware decorative animation. Use when building gradient-driven backgrounds, buttons and accents in a React project.
---

# Gradient Design: React and Tailwind Implementation

## Tailwind configuration (tailwind.config.js)
```js
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: ["./src/**/*.{js,jsx,ts,tsx}"],
  theme: {
    extend: {
      backgroundImage: {
        brand: "linear-gradient(135deg, #3b5bdb 0%, #7048e8 100%)",
        mesh:
          "radial-gradient(at 20% 20%, #a5b8ff 0, transparent 50%), radial-gradient(at 80% 30%, #9ed8ff 0, transparent 50%), radial-gradient(at 50% 90%, #d6b8ff 0, transparent 55%)",
      },
      keyframes: {
        drift: {
          "0%": { transform: "translate3d(-4%, 0, 0) rotate(0deg)" },
          "100%": { transform: "translate3d(4%, -3%, 0) rotate(6deg)" },
        },
      },
      animation: {
        drift: "drift 18s ease-in-out infinite alternate",
      },
    },
  },
  plugins: [],
};
```

## Gradient button and text (GradientButton.tsx)
```tsx
import type { ButtonHTMLAttributes } from "react";

export function GradientButton({ className = "", ...rest }: ButtonHTMLAttributes<HTMLButtonElement>) {
  return (
    <button
      className={[
        "min-h-11 rounded-xl bg-brand px-6 font-semibold text-white",
        "shadow-[0_6px_18px_rgba(88,80,236,0.35)] transition-[transform,box-shadow] duration-150",
        "hover:-translate-y-px hover:shadow-[0_10px_24px_rgba(88,80,236,0.45)]",
        "focus-visible:outline-[3px] focus-visible:outline-offset-3 focus-visible:outline-ink",
        "disabled:cursor-not-allowed disabled:opacity-50",
        "motion-reduce:transition-none",
        className,
      ].join(" ")}
      {...rest}
    />
  );
}

export function GradientText({ children }: { children: React.ReactNode }) {
  return (
    <span className="bg-brand bg-clip-text text-transparent">{children}</span>
  );
}
```

## Mesh hero (GradientHero.tsx)
```tsx
import { GradientButton } from "./GradientButton";

export default function GradientHero() {
  return (
    <header className="relative isolate grid min-h-[70vh] place-items-center overflow-hidden bg-mesh bg-slate-50 px-6 py-16">
      <div
        aria-hidden
        className="pointer-events-none absolute -inset-[20%] -z-10 bg-brand opacity-10 blur-[60px] motion-safe:animate-drift"
      />
      <section className="max-w-xl rounded-3xl bg-white/85 p-8 shadow-lg backdrop-blur-sm">
        <h1 className="mb-3 text-4xl font-bold tracking-tight text-ink md:text-5xl">
          Gradients that guide the eye
        </h1>
        <p className="mb-5 leading-relaxed text-slate-600">
          Used for emphasis and atmosphere, with text kept on a calm surface.
        </p>
        <GradientButton>Start now</GradientButton>
      </section>
    </header>
  );
}
```

## Rules for implementation
- Keep text off saturated gradient areas; place it on a scrimmed or solid surface.
- Use `motion-safe:` for decorative animation so reduced-motion users see a still background.
- Declare a solid background fallback alongside `bg-mesh`.
- Keep gradient tokens in the Tailwind config, not in components.
