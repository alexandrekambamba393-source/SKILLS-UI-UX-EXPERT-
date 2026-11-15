---
name: scroll-reveal-code-react-tailwind
description: Production-ready React and Tailwind CSS scroll reveal using a reusable Reveal component backed by IntersectionObserver, staggered children, one-time reveal and reduced-motion support. Use when adding scroll-triggered entrance animations to a React project.
---

# Scroll Reveal: React and Tailwind Implementation

## Reveal hook and component (Reveal.tsx)
```tsx
import { useEffect, useRef, useState, type ReactNode, type CSSProperties } from "react";

interface RevealProps {
  children: ReactNode;
  delay?: number;
  as?: "div" | "section" | "article" | "h2" | "p";
  className?: string;
}

export function Reveal({ children, delay = 0, as: Tag = "div", className = "" }: RevealProps) {
  const ref = useRef<HTMLElement>(null);
  const [visible, setVisible] = useState(false);

  useEffect(() => {
    const el = ref.current;
    if (!el) return;

    const reduce = window.matchMedia("(prefers-reduced-motion: reduce)").matches;
    if (reduce || !("IntersectionObserver" in window)) {
      setVisible(true);
      return;
    }

    const observer = new IntersectionObserver(
      ([entry]) => {
        if (entry.isIntersecting) {
          setVisible(true);
          observer.disconnect();
        }
      },
      { threshold: 0.15, rootMargin: "0px 0px -8% 0px" }
    );

    observer.observe(el);
    return () => observer.disconnect();
  }, []);

  const style: CSSProperties = { transitionDelay: `${delay}ms` };

  return (
    <Tag
      ref={ref as never}
      style={style}
      className={[
        "transition-[opacity,transform] duration-500 ease-out will-change-[opacity,transform]",
        visible ? "translate-y-0 opacity-100" : "translate-y-6 opacity-0",
        "motion-reduce:translate-y-0 motion-reduce:opacity-100 motion-reduce:transition-none",
        className,
      ].join(" ")}
    >
      {children}
    </Tag>
  );
}
```

## Page (ScrollRevealPage.tsx)
```tsx
import { Reveal } from "./Reveal";

const features = [
  { title: "Clear onboarding", body: "New members are productive within their first day." },
  { title: "Reliable delivery", body: "Releases ship on schedule with fewer rollbacks." },
  { title: "Transparent pricing", body: "Every cost is listed before you commit." },
];

export default function ScrollRevealPage() {
  return (
    <main className="font-sans text-ink">
      <section className="mx-auto min-h-[60vh] max-w-5xl px-6 py-24">
        <Reveal as="h2" className="mb-8 text-4xl font-semibold tracking-tight">
          Why teams choose us
        </Reveal>
        <div className="grid gap-6 sm:grid-cols-3">
          {features.map((f, i) => (
            <Reveal key={f.title} as="article" delay={i * 100} className="rounded-2xl bg-slate-100 p-7">
              <h3 className="mb-2 text-lg font-semibold">{f.title}</h3>
              <p className="leading-relaxed text-slate-600">{f.body}</p>
            </Reveal>
          ))}
        </div>
      </section>

      <section className="mx-auto min-h-[40vh] max-w-5xl px-6 py-24">
        <Reveal as="h2" className="mb-4 text-4xl font-semibold tracking-tight">
          Get started today
        </Reveal>
        <Reveal as="p" className="text-slate-600">
          Create an account in under two minutes and invite your team.
        </Reveal>
      </section>
    </main>
  );
}
```

## Rules for implementation
- Disconnect the observer after the first reveal so each element animates once.
- Keep stagger delays in one place (`i * 100`) and cap the total group length.
- Use `motion-reduce:` variants so reduced-motion users see content immediately.
- Never hide content from assistive technology; the opacity transition only affects presentation.
