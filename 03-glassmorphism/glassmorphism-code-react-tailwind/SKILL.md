---
name: glassmorphism-code-react-tailwind
description: Production-ready React and Tailwind CSS glassmorphism components, including a reusable GlassCard, a layered gradient backdrop, a solid fallback for unsupported browsers and reduced-transparency support. Use when building frosted-glass UI components in a React project.
---

# Glassmorphism: React and Tailwind Implementation

## Tailwind configuration (tailwind.config.js)
```js
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: ["./src/**/*.{js,jsx,ts,tsx}"],
  theme: {
    extend: {
      backdropBlur: {
        glass: "18px",
      },
      colors: {
        night: "#1b1f3b",
        glow: "#ffd166",
      },
      boxShadow: {
        glass: "0 8px 32px rgba(10, 14, 30, 0.25), inset 0 1px 0 rgba(255, 255, 255, 0.25)",
      },
    },
  },
  plugins: [],
};
```

## Reusable glass card (GlassCard.tsx)
```tsx
import type { HTMLAttributes, ReactNode } from "react";

interface GlassCardProps extends HTMLAttributes<HTMLElement> {
  as?: "section" | "div" | "article";
  children: ReactNode;
}

// Translucent fill, blurred backdrop, light edge. Falls back to solid colour
// when backdrop-filter is unsupported or the user prefers less transparency.
export function GlassCard({ as: Tag = "section", className = "", children, ...rest }: GlassCardProps) {
  return (
    <Tag
      className={[
        "rounded-2xl border border-white/30 bg-white/15 shadow-glass",
        "supports-[not(backdrop-filter:blur(1px))]:bg-[#141832]/85",
        "backdrop-blur-glass backdrop-saturate-150",
        "motion-reduce:backdrop-blur-none motion-reduce:bg-[#1f2547]",
        className,
      ].join(" ")}
      {...rest}
    >
      {children}
    </Tag>
  );
}
```

## Backdrop and usage (BalancePanel.tsx)
```tsx
import { GlassCard } from "./GlassCard";

function Stat({ label, value }: { label: string; value: string }) {
  return (
    <div className="flex items-center justify-between rounded-xl border border-white/30 bg-white/25 px-4 py-3.5">
      <span>{label}</span>
      <strong className="text-xl">{value}</strong>
    </div>
  );
}

export default function BalancePanel() {
  return (
    <main className="grid min-h-screen place-items-center bg-night p-6 text-white">
      {/* Layered colour blobs behind the glass give it something to refract */}
      <div
        aria-hidden
        className="pointer-events-none fixed inset-0 -z-10 bg-[radial-gradient(circle_at_15%_20%,#7b5cff,transparent_40%),radial-gradient(circle_at_85%_30%,#00c2ff,transparent_38%),radial-gradient(circle_at_50%_90%,#ff6fa8,transparent_42%)]"
      />

      <GlassCard aria-labelledby="balance-title" className="grid w-full max-w-md gap-4 p-7">
        <h2 id="balance-title" className="text-2xl font-semibold">Monthly balance</h2>
        <p className="text-white/80">Your spending is 12 percent below the previous month.</p>
        <Stat label="Available" value="$4,280" />
        <Stat label="Savings goal" value="68%" />
        <button className="min-h-11 rounded-xl bg-glow px-5 font-bold text-night transition-transform duration-150 hover:-translate-y-px focus-visible:outline-2 focus-visible:outline-offset-3 focus-visible:outline-white">
          Review budget
        </button>
      </GlassCard>
    </main>
  );
}
```

## Rules for implementation
- Keep blur between 12px and 24px (`backdrop-blur-glass` is set to 18px).
- Limit nested glass layers to two; avoid stacking them.
- Keep the `supports-[...]` fallback and the `motion-reduce` override; they cover unsupported browsers and users who need less transparency.
- Check text contrast against both the lightest and darkest parts of the backdrop.
