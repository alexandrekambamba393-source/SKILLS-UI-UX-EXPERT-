---
name: custom-cursor-code-react-tailwind
description: Production-ready React and Tailwind CSS custom cursor with a hook-based lerp follower, state modes for hover and drag, fine-pointer-only activation, pointer-events safety and reduced-motion handling. Use when adding an enhanced desktop pointer to a React project.
---

# Custom Cursor: React and Tailwind Implementation

## Cursor hook (useCustomCursor.ts)
```tsx
import { useEffect, useRef, useState } from "react";

export type CursorMode = "default" | "hover" | "drag";

export function useCustomCursor() {
  const ref = useRef<HTMLDivElement>(null);
  const [mode, setMode] = useState<CursorMode>("default");
  const [label, setLabel] = useState("");
  const [enabled, setEnabled] = useState(false);

  useEffect(() => {
    const fine = window.matchMedia("(pointer: fine)").matches;
    const reduce = window.matchMedia("(prefers-reduced-motion: reduce)").matches;
    if (!fine) return;
    setEnabled(true);

    let x = 0, y = 0, cx = 0, cy = 0, frame = 0;
    const ease = reduce ? 1 : 0.2;

    const onMove = (e: PointerEvent) => {
      x = e.clientX;
      y = e.clientY;
      if (ref.current) ref.current.style.opacity = "1";
    };

    const tick = () => {
      cx += (x - cx) * ease;
      cy += (y - cy) * ease;
      if (ref.current) ref.current.style.transform = `translate3d(${cx}px, ${cy}px, 0)`;
      frame = requestAnimationFrame(tick);
    };

    const onOver = (e: PointerEvent) => {
      const el = (e.target as HTMLElement).closest<HTMLElement>("[data-cursor]");
      const next = (el?.dataset.cursor as CursorMode) ?? "default";
      setMode(next === "hover" || next === "drag" ? next : "default");
      setLabel(next === "drag" ? "Drag" : "");
    };

    window.addEventListener("pointermove", onMove, { passive: true });
    document.addEventListener("pointerover", onOver);
    frame = requestAnimationFrame(tick);
    document.documentElement.classList.add("has-custom-cursor");

    return () => {
      cancelAnimationFrame(frame);
      window.removeEventListener("pointermove", onMove);
      document.removeEventListener("pointerover", onOver);
      document.documentElement.classList.remove("has-custom-cursor");
    };
  }, []);

  return { ref, mode, label, enabled };
}
```

## Cursor component (CustomCursor.tsx)
```tsx
import { useCustomCursor } from "./useCustomCursor";

const modeClasses = {
  default: "h-3.5 w-3.5 -mt-[7px] -ml-[7px] bg-ink mix-blend-difference",
  hover: "h-14 w-14 -mt-7 -ml-7 border-2 border-ink bg-transparent",
  drag: "h-[84px] w-[84px] -mt-[42px] -ml-[42px] bg-brand text-white text-[13px] font-bold grid place-items-center",
};

export function CustomCursor() {
  const { ref, mode, label, enabled } = useCustomCursor();
  if (!enabled) return null;

  return (
    <div
      ref={ref}
      aria-hidden="true"
      className={`pointer-events-none fixed left-0 top-0 z-[9999] rounded-full opacity-0 transition-[width,height,margin,background-color] duration-200 ease-out motion-reduce:transition-none ${modeClasses[mode]}`}
    >
      {label}
    </div>
  );
}
```

## Usage (CursorPage.tsx)
```tsx
import { CustomCursor } from "./CustomCursor";

export default function CursorPage() {
  return (
    <main className="mx-auto grid max-w-4xl gap-10 px-6 py-32 font-sans text-ink [&_.cursor-none]:cursor-none">
      <CustomCursor />
      <h1 className="text-6xl font-bold tracking-tight">Selected work</h1>
      <a href="#project-one" data-cursor="hover" className="w-fit border-b-2 border-ink text-2xl no-underline focus-visible:outline-[3px] focus-visible:outline-offset-4 focus-visible:outline-brand">
        Project One: Brand identity
      </a>
      <div data-cursor="drag" tabIndex={0} aria-label="Image gallery, drag to explore" className="h-56 rounded-2xl bg-[#e6e1d6] focus-visible:outline-[3px] focus-visible:outline-offset-4 focus-visible:outline-brand" />
      <button type="button" data-cursor="hover" className="w-fit min-h-11 rounded-full border-2 border-ink px-5 font-semibold focus-visible:outline-[3px] focus-visible:outline-offset-4 focus-visible:outline-brand">
        Start a project
      </button>
    </main>
  );
}
```

## Rules for implementation
- Activate only for `(pointer: fine)` and disable on touch devices.
- Keep `pointer-events-none` on the cursor element so it never intercepts clicks.
- Remove smoothing under reduced motion (`ease = 1`).
- Preserve the native I-beam on text inputs and editable areas.
