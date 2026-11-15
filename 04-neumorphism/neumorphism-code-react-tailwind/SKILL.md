---
name: neumorphism-code-react-tailwind
description: Production-ready React and Tailwind CSS neumorphic components using custom shadow utilities, a raised button, an accessible toggle switch and a reusable soft panel. Use when building soft-relief UI controls in a React project.
---

# Neumorphism: React and Tailwind Implementation

## Tailwind configuration (tailwind.config.js)
```js
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: ["./src/**/*.{js,jsx,ts,tsx}"],
  theme: {
    extend: {
      colors: {
        soft: "#e0e5ec",
        "soft-ink": "#2d3440",
        "soft-accent": "#4f6ef7",
      },
      boxShadow: {
        raised: "6px 6px 12px #a3b1c6, -6px -6px 12px #ffffff",
        "raised-hover": "8px 8px 16px #a3b1c6, -8px -8px 16px #ffffff",
        pressed: "inset 4px 4px 8px #a3b1c6, inset -4px -4px 8px #ffffff",
        panel: "10px 10px 20px #a3b1c6, -10px -10px 20px #ffffff",
      },
    },
  },
  plugins: [],
};
```

## Soft button (SoftButton.tsx)
```tsx
import type { ButtonHTMLAttributes } from "react";

export function SoftButton({ className = "", disabled, ...rest }: ButtonHTMLAttributes<HTMLButtonElement>) {
  return (
    <button
      disabled={disabled}
      className={[
        "min-h-11 rounded-2xl bg-soft px-5 font-semibold text-soft-ink",
        "shadow-raised transition-[box-shadow,transform] duration-200 ease-out",
        "hover:shadow-raised-hover active:translate-y-px active:shadow-pressed",
        "focus-visible:outline-2 focus-visible:outline-offset-4 focus-visible:outline-soft-accent",
        "disabled:cursor-not-allowed disabled:opacity-50 disabled:shadow-none",
        "motion-reduce:transition-none",
        className,
      ].join(" ")}
      {...rest}
    />
  );
}
```

## Toggle switch (SoftToggle.tsx)
```tsx
import { useId, useState } from "react";

export function SoftToggle({ label, defaultChecked = false }: { label: string; defaultChecked?: boolean }) {
  const id = useId();
  const [checked, setChecked] = useState(defaultChecked);

  return (
    <div className="flex items-center justify-between text-soft-ink">
      <label htmlFor={id}>{label}</label>
      <button
        id={id}
        role="switch"
        aria-checked={checked}
        onClick={() => setChecked((c) => !c)}
        className="relative inline-flex h-8 w-16 items-center rounded-full bg-soft shadow-pressed focus-visible:outline-2 focus-visible:outline-offset-4 focus-visible:outline-soft-accent"
      >
        <span
          className={[
            "ml-1 h-6 w-6 rounded-full bg-soft shadow-raised transition-transform duration-[250ms] motion-reduce:transition-none",
            checked ? "translate-x-8 bg-soft-accent" : "",
          ].join(" ")}
        />
      </button>
    </div>
  );
}
```

## Soft panel (SettingsPanel.tsx)
```tsx
import { SoftButton } from "./SoftButton";
import { SoftToggle } from "./SoftToggle";

export default function SettingsPanel() {
  return (
    <main className="grid min-h-screen place-items-center bg-soft p-6">
      <section aria-label="Device settings" className="grid w-full max-w-sm gap-6 rounded-3xl bg-soft p-8 shadow-panel">
        <SoftToggle label="Wi-Fi" defaultChecked />
        <SoftButton>Save changes</SoftButton>
        <SoftButton disabled>Unavailable</SoftButton>
      </section>
    </main>
  );
}
```

## Rules for implementation
- Keep all surfaces on the same `bg-soft` token; shadows must match that colour.
- Use `role="switch"` with `aria-checked` for toggles, not a styled div.
- Keep focus rings solid and outside the shadow system.
