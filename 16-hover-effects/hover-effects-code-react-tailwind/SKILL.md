---
name: hover-effects-code-react-tailwind
description: Production-ready React and Tailwind CSS hover effects for cards, buttons, underlined links and list rows that reveal actions on hover and focus, using hover-capable variants and reduced-motion support. Use when building accessible hover interactions in a React project.
---

# Hover Effects: React and Tailwind Implementation

## Card component (HoverCard.tsx)
```tsx
import type { ReactNode } from "react";

export function HoverCard({ title, href, children }: { title: string; href: string; children: ReactNode }) {
  return (
    <article className="group relative rounded-2xl border border-slate-200 bg-white p-6 transition-[transform,box-shadow] duration-200 ease-out hover:-translate-y-0.5 hover:shadow-lg focus-within:outline-2 focus-within:outline-offset-3 focus-within:outline-brand motion-reduce:transition-none motion-reduce:hover:translate-y-0 [@media(hover:none)]:hover:translate-y-0">
      <h2 className="mb-2 text-lg font-semibold">
        <a href={href} className="text-ink no-underline after:absolute after:inset-0 after:rounded-2xl focus-visible:outline-none">
          {title}
        </a>
      </h2>
      <div className="leading-relaxed text-slate-600">{children}</div>
    </article>
  );
}
```

## Underline link and tinted button (HoverControls.tsx)
```tsx
export function UnderlineLink({ href, children }: { href: string; children: React.ReactNode }) {
  return (
    <a
      href={href}
      className="bg-[linear-gradient(currentColor,currentColor)] bg-[length:0%_1px] bg-no-repeat bg-[position:0_100%] pb-0.5 text-brand no-underline transition-[background-size] duration-200 ease-out hover:bg-[length:100%_1px] focus-visible:bg-[length:100%_1px] focus-visible:outline-2 focus-visible:outline-offset-3 focus-visible:outline-brand motion-reduce:transition-none"
    >
      {children}
    </a>
  );
}

export function TintButton({ children, ...rest }: React.ButtonHTMLAttributes<HTMLButtonElement>) {
  return (
    <button
      type="button"
      className="min-h-11 rounded-xl border border-slate-300 bg-white px-5 font-semibold text-ink transition-[background-color,border-color,transform] duration-150 ease-out hover:border-brand hover:bg-brand-soft active:scale-[0.98] focus-visible:outline-2 focus-visible:outline-offset-3 focus-visible:outline-brand motion-reduce:transition-none"
      {...rest}
    >
      {children}
    </button>
  );
}
```

## List row with revealed action (ProjectRow.tsx)
```tsx
export function ProjectRow({ name, onOpen }: { name: string; onOpen: () => void }) {
  return (
    <div className="group flex items-center justify-between rounded-xl bg-white px-4 py-3.5 transition-colors duration-150 hover:bg-brand-soft focus-within:bg-brand-soft">
      <span className="font-medium">{name}</span>
      <button
        type="button"
        onClick={onOpen}
        className="min-h-10 px-2 font-semibold text-brand opacity-55 transition-opacity duration-150 group-hover:opacity-100 focus-visible:opacity-100 focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-brand motion-reduce:transition-none"
      >
        Open
      </button>
    </div>
  );
}
```

## Rules for implementation
- Use `hover:` variants together with `focus-visible:` or `focus-within:` counterparts.
- Use the `[@media(hover:none)]` override so touch devices do not keep lifted states.
- Reveal secondary actions through `group-hover` and `focus-visible` together.
- Disable movement with `motion-reduce:` variants.
