---
name: design-system-code-react-tailwind
description: Production-ready React and Tailwind CSS design system with a Tailwind theme wired to CSS variable semantic tokens, a theme provider, a Button component with complete states and a Card component using only semantic utilities. Use when building a token-driven component library in a React project.
---

# Design System: React and Tailwind Implementation

## Tokens and Tailwind theme (tailwind.config.js)
```js
/** @type {import('tailwindcss').Config} */
module.exports = {
  darkMode: "class",
  content: ["./src/**/*.{js,jsx,ts,tsx}"],
  theme: {
    extend: {
      colors: {
        // Semantic tokens: components use these names only
        bg: "rgb(var(--color-bg) / <alpha-value>)",
        surface: "rgb(var(--color-surface) / <alpha-value>)",
        text: "rgb(var(--color-text) / <alpha-value>)",
        muted: "rgb(var(--color-text-muted) / <alpha-value>)",
        border: "rgb(var(--color-border) / <alpha-value>)",
        action: "rgb(var(--color-action) / <alpha-value>)",
        "action-hover": "rgb(var(--color-action-hover) / <alpha-value>)",
        "action-text": "rgb(var(--color-action-text) / <alpha-value>)",
        focus: "rgb(var(--color-focus) / <alpha-value>)",
        danger: "rgb(var(--color-danger) / <alpha-value>)",
      },
      borderRadius: {
        sm: "8px",
        md: "12px",
        lg: "16px",
      },
      transitionDuration: {
        fast: "150ms",
        base: "220ms",
      },
    },
  },
  plugins: [],
};
```

## Global theme variables (index.css)
```css
@tailwind base;
@tailwind components;
@tailwind utilities;

/* Primitive values are defined once, semantic values change per theme */
:root {
  color-scheme: light;
  --color-bg: 247 248 250;
  --color-surface: 255 255 255;
  --color-text: 16 24 40;
  --color-text-muted: 71 84 103;
  --color-border: 208 213 221;
  --color-action: 47 91 234;
  --color-action-hover: 36 72 196;
  --color-action-text: 255 255 255;
  --color-focus: 47 91 234;
  --color-danger: 180 35 24;
}

.dark {
  color-scheme: dark;
  --color-bg: 14 17 22;
  --color-surface: 22 27 34;
  --color-text: 232 235 239;
  --color-text-muted: 154 163 174;
  --color-border: 44 52 63;
  --color-action: 106 160 255;
  --color-action-hover: 143 184 255;
  --color-action-text: 11 18 32;
  --color-focus: 143 184 255;
  --color-danger: 255 138 128;
}
```

## Theme provider (ThemeProvider.tsx)
```tsx
import { createContext, useContext, useEffect, useState, type ReactNode } from "react";

type Theme = "light" | "dark";
const Ctx = createContext<{ theme: Theme; setTheme: (t: Theme) => void } | null>(null);

export function ThemeProvider({ children }: { children: ReactNode }) {
  const [theme, setTheme] = useState<Theme>(() => {
    try {
      const saved = localStorage.getItem("ds-theme") as Theme | null;
      if (saved) return saved;
    } catch { /* ignore */ }
    return window.matchMedia("(prefers-color-scheme: dark)").matches ? "dark" : "light";
  });

  useEffect(() => {
    document.documentElement.classList.toggle("dark", theme === "dark");
    try { localStorage.setItem("ds-theme", theme); } catch { /* ignore */ }
  }, [theme]);

  return <Ctx.Provider value={{ theme, setTheme }}>{children}</Ctx.Provider>;
}

export function useTheme() {
  const v = useContext(Ctx);
  if (!v) throw new Error("useTheme must be used inside ThemeProvider");
  return v;
}
```

## Button component (Button.tsx)
```tsx
import { forwardRef, type ButtonHTMLAttributes } from "react";

type Variant = "primary" | "secondary";

interface ButtonProps extends ButtonHTMLAttributes<HTMLButtonElement> {
  variant?: Variant;
  loading?: boolean;
}

const variants: Record<Variant, string> = {
  primary: "bg-action text-action-text hover:bg-action-hover",
  secondary: "border border-border bg-transparent text-text hover:bg-bg",
};

export const Button = forwardRef<HTMLButtonElement, ButtonProps>(
  ({ variant = "primary", loading = false, disabled, className = "", children, ...rest }, ref) => (
    <button
      ref={ref}
      disabled={disabled || loading}
      aria-busy={loading || undefined}
      className={[
        "inline-flex min-h-11 items-center justify-center rounded-sm px-6 font-semibold",
        "transition-[background-color,transform] duration-fast ease-out active:translate-y-px",
        "focus-visible:outline-[3px] focus-visible:outline-offset-3 focus-visible:outline-focus",
        "disabled:cursor-not-allowed disabled:opacity-50",
        "aria-busy:cursor-progress motion-reduce:transition-none",
        variants[variant],
        className,
      ].join(" ")}
      {...rest}
    >
      {loading ? "Loading..." : children}
    </button>
  )
);
Button.displayName = "Button";
```

## Card component (Card.tsx)
```tsx
import type { ReactNode } from "react";

export function Card({ title, children, actions }: { title: string; children: ReactNode; actions?: ReactNode }) {
  return (
    <article className="grid w-full max-w-sm gap-4 rounded-lg border border-border bg-surface p-6 text-text">
      <h2 className="text-xl font-semibold">{title}</h2>
      <div className="leading-relaxed text-muted">{children}</div>
      {actions && <div className="flex flex-wrap gap-3">{actions}</div>}
    </article>
  );
}
```

## Usage (DesignSystemDemo.tsx)
```tsx
import { Button } from "./Button";
import { Card } from "./Card";
import { useTheme } from "./ThemeProvider";

export default function DesignSystemDemo() {
  const { theme, setTheme } = useTheme();

  return (
    <main className="grid min-h-screen place-items-center bg-bg p-6 font-sans">
      <div className="grid gap-4">
        <Button variant="secondary" aria-pressed={theme === "dark"} onClick={() => setTheme(theme === "dark" ? "light" : "dark")}>
          {theme === "dark" ? "Light theme" : "Dark theme"}
        </Button>

        <Card
          title="Project summary"
          actions={
            <>
              <Button>Primary action</Button>
              <Button variant="secondary">Secondary</Button>
            </>
          }
        >
          Tokens drive every colour and spacing decision. Switch themes to see semantic values change without touching components.
        </Card>
      </div>
    </main>
  );
}
```

## Rules for implementation
- Components use semantic utilities (`bg-surface`, `text-muted`) only; never raw palette colours.
- Theme by changing CSS variables under `.dark`, not by rewriting components.
- Every component supports its full state set, including focus, disabled and busy.
- Version the component API and document breaking changes.
