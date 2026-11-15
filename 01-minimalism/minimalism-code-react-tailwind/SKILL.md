---
name: minimalism-code-react-tailwind
description: Production-ready React and Tailwind CSS code for minimalist interfaces, with a restrained button component, a single-focus content layout and an 8px spacing system expressed as Tailwind utilities. Use when implementing a minimal UI in a React project.
---

# Minimalism: React and Tailwind Implementation

## Tailwind configuration (tailwind.config.js)
```js
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: ["./src/**/*.{js,jsx,ts,tsx}"],
  theme: {
    extend: {
      colors: {
        canvas: "#fafafa",
        ink: "#111111",
        muted: "#6b6b6b",
        line: "#e6e6e6",
        accent: { DEFAULT: "#2563eb", hover: "#1d4ed8" },
      },
      fontFamily: {
        sans: ["Inter", "system-ui", "-apple-system", "Segoe UI", "sans-serif"],
      },
      spacing: {
        // 8px base scale is already provided by Tailwind (2 = 8px, 4 = 16px ...)
      },
      transitionTimingFunction: {
        calm: "cubic-bezier(0.2, 0.6, 0.2, 1)",
      },
    },
  },
  plugins: [],
};
```

## Button component (Button.tsx)
```tsx
import { forwardRef, type ButtonHTMLAttributes } from "react";

type Variant = "primary" | "ghost";

interface ButtonProps extends ButtonHTMLAttributes<HTMLButtonElement> {
  variant?: Variant;
  loading?: boolean;
}

const base =
  "inline-flex min-h-11 items-center justify-center rounded-lg px-6 font-semibold " +
  "transition-colors duration-200 ease-calm motion-reduce:transition-none " +
  "focus-visible:outline-2 focus-visible:outline-offset-3 focus-visible:outline-accent " +
  "disabled:cursor-not-allowed disabled:opacity-40";

const variants: Record<Variant, string> = {
  primary: "bg-accent text-white hover:bg-accent-hover",
  ghost: "bg-transparent text-ink hover:bg-line",
};

export const Button = forwardRef<HTMLButtonElement, ButtonProps>(
  ({ variant = "primary", loading = false, className = "", children, disabled, ...rest }, ref) => (
    <button
      ref={ref}
      disabled={disabled || loading}
      aria-busy={loading || undefined}
      className={`${base} ${variants[variant]} ${className}`}
      {...rest}
    >
      {loading ? "Loading..." : children}
    </button>
  )
);
Button.displayName = "Button";
```

## Minimal screen (LandingScreen.tsx)
```tsx
import { Button } from "./Button";

export default function LandingScreen() {
  return (
    <main className="mx-auto grid max-w-xl gap-8 bg-canvas px-6 py-16 font-sans text-ink">
      <header className="grid gap-2">
        <h1 className="text-3xl font-semibold tracking-tight">One clear message</h1>
        <p className="text-muted">A single sentence that explains the value of this screen.</p>
      </header>

      <div className="flex flex-wrap gap-4">
        <Button>Get started</Button>
        <Button variant="ghost">Learn more</Button>
      </div>
    </main>
  );
}
```

## Rules for implementation
- Use the tokens defined in `tailwind.config.js`; avoid arbitrary values such as `text-[13px]`.
- Keep one accent colour. Do not add a second brand colour through props.
- Use `motion-reduce:` variants to respect reduced-motion preferences.
- Every interactive element keeps a minimum 44px height (`min-h-11`).
