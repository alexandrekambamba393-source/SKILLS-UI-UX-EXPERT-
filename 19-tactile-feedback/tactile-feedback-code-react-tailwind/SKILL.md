---
name: tactile-feedback-code-react-tailwind
description: Production-ready React and Tailwind CSS tactile button with press and release states, slide-to-cancel handling, optional haptics through the Vibration API and reduced-motion support. Use when building touch-responsive controls in a React project.
---

# Tactile Feedback: React and Tailwind Implementation

## Haptics helper (haptics.ts)
```ts
export type HapticKind = "light" | "medium" | "success";

const patterns: Record<HapticKind, number | number[]> = {
  light: 8,
  medium: 18,
  success: [12, 40, 12],
};

// Optional enhancement. Unsupported on many browsers (including iOS Safari).
export function haptic(kind: HapticKind) {
  if (typeof navigator !== "undefined" && "vibrate" in navigator) {
    navigator.vibrate(patterns[kind]);
  }
}
```

## Tactile button (TactileButton.tsx)
```tsx
import { useRef, useState, type ButtonHTMLAttributes } from "react";
import { haptic, type HapticKind } from "./haptics";

interface TactileButtonProps extends ButtonHTMLAttributes<HTMLButtonElement> {
  hapticKind?: HapticKind;
}

export function TactileButton({ hapticKind = "light", className = "", onClick, disabled, children, ...rest }: TactileButtonProps) {
  const [pressed, setPressed] = useState(false);
  const cancelled = useRef(false);

  return (
    <button
      type="button"
      disabled={disabled}
      onPointerDown={(e) => {
        cancelled.current = false;
        setPressed(true);
        e.currentTarget.setPointerCapture(e.pointerId);
      }}
      onPointerMove={(e) => {
        const r = e.currentTarget.getBoundingClientRect();
        const inside = e.clientX >= r.left && e.clientX <= r.right && e.clientY >= r.top && e.clientY <= r.bottom;
        if (!inside) {
          cancelled.current = true;
          setPressed(false);
        }
      }}
      onPointerUp={() => {
        setPressed(false);
        if (!cancelled.current) haptic(hapticKind);
      }}
      onPointerCancel={() => setPressed(false)}
      onPointerLeave={() => setPressed(false)}
      onClick={(e) => {
        if (!cancelled.current) onClick?.(e);
      }}
      className={[
        "min-h-[52px] select-none touch-manipulation rounded-2xl bg-brand px-5 font-semibold text-white",
        "shadow-[0_4px_10px_rgba(59,91,219,0.25)] transition-[transform,background-color,box-shadow] duration-150 ease-out",
        pressed ? "scale-[0.97] bg-brand-hover shadow-[0_2px_4px_rgba(59,91,219,0.2)]" : "",
        "active:scale-[0.97] focus-visible:outline-[3px] focus-visible:outline-offset-3 focus-visible:outline-ink",
        "disabled:cursor-not-allowed disabled:opacity-50 disabled:scale-100",
        "motion-reduce:transition-colors motion-reduce:scale-100 motion-reduce:active:scale-100",
        className,
      ].join(" ")}
      {...rest}
    >
      {children}
    </button>
  );
}
```

## Usage (Checkout.tsx)
```tsx
import { useState } from "react";
import { TactileButton } from "./TactileButton";

export default function Checkout() {
  const [status, setStatus] = useState("");

  async function save() {
    setStatus("Saving...");
    await new Promise((r) => setTimeout(r, 900));
    setStatus("Changes saved.");
  }

  return (
    <main className="mx-auto grid w-[min(360px,92vw)] gap-4 py-24 font-sans text-ink">
      <TactileButton>Select plan</TactileButton>
      <TactileButton hapticKind="medium" onClick={save}>Save changes</TactileButton>
      <p role="status" aria-live="polite" className="min-h-6 text-center text-sm text-slate-500">{status}</p>
    </main>
  );
}
```

## Rules for implementation
- Keep press feedback under 100ms and release under 200ms.
- Cancel the click when the pointer slides off the control.
- Treat haptics as optional; never rely on them to signal state.
- Preserve colour feedback under reduced motion.
