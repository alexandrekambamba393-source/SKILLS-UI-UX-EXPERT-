---
name: intuitive-navigation-code-react-tailwind
description: Production-ready React and Tailwind CSS navigation with a primary nav using aria-current, a breadcrumb trail, a responsive menu with Escape handling and focus return, and a search box with keyboard-accessible suggestions. Use when building site or app navigation in a React project.
---

# Intuitive Navigation: React and Tailwind Implementation

## Primary navigation (PrimaryNav.tsx)
```tsx
import { useEffect, useRef, useState } from "react";

const items = [
  { href: "/guides", label: "Guides", current: true },
  { href: "/api", label: "API reference" },
  { href: "/changelog", label: "Changelog" },
  { href: "/support", label: "Support" },
];

export function PrimaryNav() {
  const [open, setOpen] = useState(false);
  const toggleRef = useRef<HTMLButtonElement>(null);

  useEffect(() => {
    if (!open) return;
    const onKey = (e: KeyboardEvent) => {
      if (e.key === "Escape") {
        setOpen(false);
        toggleRef.current?.focus();
      }
    };
    document.addEventListener("keydown", onKey);
    return () => document.removeEventListener("keydown", onKey);
  }, [open]);

  return (
    <>
      <button
        ref={toggleRef}
        type="button"
        aria-expanded={open}
        aria-controls="primary-nav"
        onClick={() => setOpen((v) => !v)}
        className="inline-flex min-h-11 items-center rounded-lg border border-slate-200 bg-white px-4 font-semibold md:hidden focus-visible:outline-[3px] focus-visible:outline-offset-3 focus-visible:outline-brand"
      >
        Menu
      </button>

      <nav
        id="primary-nav"
        aria-label="Primary"
        className={`${open ? "block" : "hidden"} absolute inset-x-0 top-full border-b border-slate-200 bg-white px-6 pb-4 pt-2 md:static md:block md:border-0 md:p-0`}
      >
        <ul className="flex flex-col gap-1 md:flex-row md:gap-1">
          {items.map((item) => (
            <li key={item.href}>
              <a
                href={item.href}
                aria-current={item.current ? "page" : undefined}
                className="flex min-h-11 items-center rounded-lg px-3 font-medium text-ink no-underline hover:bg-slate-100 aria-[current=page]:bg-[#eef2ff] aria-[current=page]:font-semibold aria-[current=page]:text-brand focus-visible:outline-[3px] focus-visible:outline-offset-3 focus-visible:outline-brand"
              >
                {item.label}
              </a>
            </li>
          ))}
        </ul>
      </nav>
    </>
  );
}
```

## Breadcrumbs (Breadcrumbs.tsx)
```tsx
export function Breadcrumbs({ trail }: { trail: { label: string; href?: string }[] }) {
  return (
    <nav aria-label="Breadcrumb" className="mx-auto max-w-6xl px-6 pt-5 text-sm text-body">
      <ol className="flex flex-wrap items-center gap-1.5">
        {trail.map((crumb, i) => {
          const last = i === trail.length - 1;
          return (
            <li key={crumb.label} className="flex items-center gap-1.5">
              {i > 0 && <span aria-hidden="true" className="text-slate-400">/</span>}
              {last || !crumb.href ? (
                <span aria-current={last ? "page" : undefined} className="text-ink">{crumb.label}</span>
              ) : (
                <a href={crumb.href} className="text-brand no-underline hover:underline">{crumb.label}</a>
              )}
            </li>
          );
        })}
      </ol>
    </nav>
  );
}
```

## Search with suggestions (SearchBox.tsx)
```tsx
import { useId, useState } from "react";

const pages = ["Getting started", "Authentication", "Deploy a project", "Environment variables", "Billing"];

export function SearchBox() {
  const [term, setTerm] = useState("");
  const listId = useId();
  const matches = term.trim() ? pages.filter((p) => p.toLowerCase().includes(term.trim().toLowerCase())) : [];
  const open = matches.length > 0;

  return (
    <div role="search" className="relative">
      <label htmlFor={`${listId}-input`} className="sr-only">Search documentation</label>
      <input
        id={`${listId}-input`}
        type="search"
        value={term}
        onChange={(e) => setTerm(e.target.value)}
        placeholder="Search docs"
        aria-autocomplete="list"
        aria-controls={listId}
        aria-expanded={open}
        className="min-h-11 w-56 rounded-lg border border-slate-200 bg-white px-3 focus-visible:outline-[3px] focus-visible:outline-offset-2 focus-visible:outline-brand"
      />
      {open && (
        <ul id={listId} className="absolute left-0 right-0 top-12 z-20 grid gap-0.5 rounded-lg border border-slate-200 bg-white p-1.5 shadow-lg">
          {matches.map((m) => (
            <li key={m}>
              <button type="button" className="min-h-10 w-full rounded-md px-2.5 text-left hover:bg-slate-100 focus-visible:bg-slate-100 focus-visible:outline-none">
                {m}
              </button>
            </li>
          ))}
        </ul>
      )}
    </div>
  );
}
```

## Header (SiteHeader.tsx)
```tsx
import { Breadcrumbs } from "./Breadcrumbs";
import { PrimaryNav } from "./PrimaryNav";
import { SearchBox } from "./SearchBox";

export default function SiteHeader() {
  return (
    <>
      <a href="#main" className="sr-only focus:not-sr-only focus:absolute focus:left-3 focus:top-3 focus:z-50 focus:rounded-lg focus:bg-ink focus:px-3 focus:py-2 focus:text-white">
        Skip to content
      </a>
      <header className="sticky top-0 z-10 border-b border-slate-200 bg-white">
        <div className="relative mx-auto flex min-h-16 max-w-6xl items-center gap-4 px-6">
          <a href="/" className="mr-auto font-bold text-ink no-underline">Atlas Docs</a>
          <SearchBox />
          <PrimaryNav />
        </div>
      </header>
      <Breadcrumbs trail={[{ label: "Home", href: "/" }, { label: "Guides", href: "/guides" }, { label: "Getting started" }]} />
    </>
  );
}
```

## Rules for implementation
- Use `aria-current="page"` for the active item in both primary navigation and breadcrumbs.
- Close the mobile menu on Escape and return focus to its toggle.
- Keep the primary nav structure identical across breakpoints; only the presentation changes.
- Put the skip link first in the rendered tree.
