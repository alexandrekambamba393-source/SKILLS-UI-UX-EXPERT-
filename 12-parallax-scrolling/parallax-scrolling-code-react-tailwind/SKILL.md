---
name: parallax-scrolling-code-react-tailwind
description: Production-ready React and Tailwind CSS parallax hero with a scroll-linked layer hook, rAF-throttled transforms, a static content plane and reduced-motion and mobile safeguards. Use when implementing layered scroll-based depth in a React project.
---

# Parallax Scrolling: React and Tailwind Implementation

## Scroll hook (useParallax.ts)
```tsx
import { useEffect, useState } from "react";

export function useReducedMotion() {
  const [reduced, setReduced] = useState(false);
  useEffect(() => {
    const mq = window.matchMedia("(prefers-reduced-motion: reduce)");
    const update = () => setReduced(mq.matches);
    update();
    mq.addEventListener("change", update);
    return () => mq.removeEventListener("change", update);
  }, []);
  return reduced;
}

// Returns the current scroll offset, updated once per animation frame.
export function useScrollY(enabled: boolean) {
  const [y, setY] = useState(0);
  useEffect(() => {
    if (!enabled) return;
    let frame = 0;
    const onScroll = () => {
      cancelAnimationFrame(frame);
      frame = requestAnimationFrame(() => setY(window.scrollY));
    };
    onScroll();
    window.addEventListener("scroll", onScroll, { passive: true });
    return () => {
      cancelAnimationFrame(frame);
      window.removeEventListener("scroll", onScroll);
    };
  }, [enabled]);
  return y;
}
```

## Parallax hero (ParallaxHero.tsx)
```tsx
import { useReducedMotion, useScrollY } from "./useParallax";

export default function ParallaxHero() {
  const reduced = useReducedMotion();
  const isWide = typeof window !== "undefined" && window.matchMedia("(min-width: 769px)").matches;
  const enabled = !reduced && isWide;
  const y = useScrollY(enabled);

  const offset = (speed: number) => (enabled ? { transform: `translate3d(0, ${(y * speed).toFixed(2)}px, 0)` } : undefined);

  return (
    <>
      <header className="relative isolate grid h-screen place-items-center overflow-hidden bg-[#1d2a3a] px-6 text-center text-white">
        <div aria-hidden className="pointer-events-none absolute -inset-[15%_-5%] -z-10 bg-[radial-gradient(circle_at_30%_30%,#3a5a80,transparent_55%)] will-change-transform" style={offset(0.2)} />
        <div aria-hidden className="pointer-events-none absolute -inset-[15%_-5%] -z-10 bg-[radial-gradient(circle_at_75%_60%,#d9480f55,transparent_50%)] will-change-transform" style={offset(0.4)} />
        <div aria-hidden className="pointer-events-none absolute -inset-[15%_-5%] -z-10 bg-[radial-gradient(circle_at_50%_80%,#ffffff22,transparent_40%)] will-change-transform" style={offset(-0.15)} />

        <div className="max-w-2xl">
          <h1 className="mb-4 text-5xl font-bold leading-tight tracking-tight md:text-7xl">Stories that unfold with depth</h1>
          <p className="text-lg leading-relaxed text-white/85">
            Scroll to see how layers move at different speeds to create a sense of space.
          </p>
        </div>
      </header>

      <section className="mx-auto max-w-2xl px-6 py-[120px]">
        <h2 className="mb-3 text-3xl font-semibold">Built to stay readable</h2>
        <p className="leading-relaxed">
          Text never moves with the background. The content plane stays fixed while decorative layers drift behind it.
        </p>
      </section>
    </>
  );
}
```

## Rules for implementation
- Keep the content plane static; only decorative layers receive transforms.
- Respect `prefers-reduced-motion` and disable motion on narrow viewports.
- Keep scroll work in one animation frame per update.
- Use `transform` only and avoid layout-triggering properties.
