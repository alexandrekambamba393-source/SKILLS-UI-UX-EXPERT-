---
name: dark-mode-code-react-tailwind
description: Production-ready React and Tailwind CSS dark mode implementation using the class strategy, a theme provider that respects system preference, a persistent toggle and flash-free initialization. Use when adding a light and dark theme to a React or Next.js project with Tailwind.
---

# Dark Mode: React and Tailwind Implementation

## Tailwind configuration (tailwind.config.js)
```js
/** @type {import('tailwindcss').Config} */
module.exports = {
  darkMode: "class",
  content: ["./src/**/*.{js,jsx,ts,tsx}"],
  theme: {
    extend: {
      colors: {
        // Semantic tokens: components use these, never raw hex values.
        bg: "rgb(var(--color-bg) / <alpha-value>)",
        surface: "rgb(var(--color-surface) / <alpha-value>)",
        raised: "rgb(var(--color-raised) / <alpha-value>)",
        border: "rgb(var(--color-border) / <alpha-value>)",
        text: "rgb(var(--color-text) / <alpha-value>)",
        muted: "rgb(var(--color-muted) / <alpha-value>)",
        accent: "rgb(var(--color-accent) / <alpha-value>)",
        "accent-fg": "rgb(var(--color-accent-fg) / <alpha-value>)",
      },
    },
  },
  plugins: [],
};
```

## Global CSS (globals.css)
```css
@tailwind base;
@tailwind components;
@tailwind utilities;

:root {
  color-scheme: light;
  --color-bg: 255 255 255;
  --color-surface: 245 246 248;
  --color-raised: 255 255 255;
  --color-border: 217 221 227;
  --color-text: 17 20 24;
  --color-muted: 91 100 112;
  --color-accent: 47 111 228;
  --color-accent-fg: 255 255 255;
}

.dark {
  color-scheme: dark;
  --color-bg: 14 17 22;
  --color-surface: 22 27 34;
  --color-raised: 30 36 44;
  --color-border: 44 52 63;
  --color-text: 232 235 239;
  --color-muted: 154 163 174;
  --color-accent: 106 160 255;
  --color-accent-fg: 11 18 32;
}
```

## Theme provider (ThemeProvider.tsx)
```tsx
import { createContext, useContext, useEffect, useState, type ReactNode } from "react";

type Theme = "light" | "dark";
const ThemeContext = createContext<{ theme: Theme; toggle: () => void } | null>(null);

function getInitialTheme(): Theme {
  try {
    const saved = localStorage.getItem("theme") as Theme | null;
    if (saved === "light" || saved === "dark") return saved;
  } catch {
    /* storage unavailable: fall through to system preference */
  }
  return window.matchMedia("(prefers-color-scheme: dark)").matches ? "dark" : "light";
}

export function ThemeProvider({ children }: { children: ReactNode }) {
  const [theme, setTheme] = useState<Theme>(getInitialTheme);

  useEffect(() => {
    document.documentElement.classList.toggle("dark", theme === "dark");
    try {
      localStorage.setItem("theme", theme);
    } catch {
      /* ignore */
    }
  }, [theme]);

  const toggle = () => setTheme((t) => (t === "dark" ? "light" : "dark"));

  return <ThemeContext.Provider value={{ theme, toggle }}>{children}</ThemeContext.Provider>;
}

export function useTheme() {
  const ctx = useContext(ThemeContext);
  if (!ctx) throw new Error("useTheme must be used inside ThemeProvider");
  return ctx;
}
```

## Flash prevention (index.html or Next.js _document)
```html
<script>
  (function () {
    try {
      var t = localStorage.getItem("theme");
      var dark = t ? t === "dark" : window.matchMedia("(prefers-color-scheme: dark)").matches;
      if (dark) document.documentElement.classList.add("dark");
    } catch (e) {}
  })();
</script>
```

## Toggle and card (DashboardCard.tsx)
```tsx
import { useTheme } from "./ThemeProvider";

export default function DashboardCard() {
  const { theme, toggle } = useTheme();

  return (
    <section className="mx-auto mt-12 max-w-md rounded-xl border border-border bg-raised p-6 text-text transition-colors duration-200 motion-reduce:transition-none">
      <h2 className="mb-2 text-xl font-semibold">Account overview</h2>
      <p className="mb-4 text-muted">Theme follows your system setting until you choose one.</p>
      <p className="mb-4 text-green-600 dark:text-green-400">Payments are up to date.</p>

      <button
        type="button"
        onClick={toggle}
        aria-pressed={theme === "dark"}
        className="min-h-11 rounded-lg bg-accent px-4 font-semibold text-accent-fg focus-visible:outline-2 focus-visible:outline-offset-3 focus-visible:outline-accent"
      >
        Switch theme
      </button>
    </section>
  );
}
```

## Rules for implementation
- Use semantic tokens (`bg-surface`, `text-muted`) rather than `bg-gray-800` so themes stay consistent.
- Only colour tokens change between themes; layout and spacing remain identical.
- Wrap the app in `ThemeProvider` and include the flash-prevention script before first paint.
- Test every state (hover, focus, error, disabled) in both themes.
