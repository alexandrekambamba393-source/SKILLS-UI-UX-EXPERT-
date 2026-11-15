---
name: page-transitions-code-react-tailwind
description: Production-ready React and Tailwind CSS page transitions using React Router with the View Transitions API, shared-element names, focus management on route change and reduced-motion handling. Use when adding smooth route transitions to a React single-page application.
---

# Page Transitions: React and Tailwind Implementation

## Route-aware transition link (TransitionLink.tsx)
```tsx
import { Link, useNavigate, type LinkProps } from "react-router-dom";
import type { MouseEvent } from "react";

// Wraps navigation in document.startViewTransition when the browser supports it.
export function TransitionLink(props: LinkProps) {
  const navigate = useNavigate();

  function onClick(e: MouseEvent<HTMLAnchorElement>) {
    if (
      !document.startViewTransition ||
      window.matchMedia("(prefers-reduced-motion: reduce)").matches ||
      e.metaKey || e.ctrlKey || e.shiftKey || e.button !== 0
    ) {
      return;
    }
    e.preventDefault();
    const href = typeof props.to === "string" ? props.to : props.to.pathname ?? "/";
    document.startViewTransition(() => navigate(href));
  }

  return <Link {...props} onClick={onClick} />;
}
```

## Focus management on route change (RouteFocus.tsx)
```tsx
import { useEffect } from "react";
import { useLocation } from "react-router-dom";

// Moves focus to the page heading after navigation so assistive technology announces the change.
export function RouteFocus() {
  const { pathname } = useLocation();

  useEffect(() => {
    const heading = document.querySelector<HTMLElement>("main h1");
    if (heading) {
      heading.setAttribute("tabindex", "-1");
      heading.focus({ preventScroll: true });
    }
    document.title = document.querySelector("main h1")?.textContent ?? document.title;
  }, [pathname]);

  return null;
}
```

## Shared-element thumbnail (ProjectCard.tsx)
```tsx
import { TransitionLink } from "./TransitionLink";

export function ProjectCard({ id, title, summary, img }: { id: string; title: string; summary: string; img: string }) {
  return (
    <TransitionLink
      to={`/projects/${id}`}
      className="flex items-center gap-4 rounded-2xl border border-slate-200 p-4 text-ink no-underline transition-colors hover:border-slate-300 focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-brand"
    >
      <img
        src={img}
        alt=""
        style={{ viewTransitionName: `thumb-${id}` }}
        className="h-24 w-24 rounded-2xl bg-slate-200 object-cover"
      />
      <div>
        <strong className="block text-lg">{title}</strong>
        <p className="mt-1 text-slate-600">{summary}</p>
      </div>
    </TransitionLink>
  );
}
```

## Detail view (ProjectDetail.tsx)
```tsx
import { Link, useParams } from "react-router-dom";

const projects: Record<string, { title: string; summary: string; img: string }> = {
  atlas: { title: "Atlas", summary: "A mapping tool for field teams that works offline.", img: "https://images.unsplash.com/photo-1507525428034-b723cf961d3e?w=1200" },
};

export default function ProjectDetail() {
  const { id = "" } = useParams();
  const p = projects[id];

  if (!p) {
    return (
      <main className="mx-auto max-w-2xl px-6 py-12">
        <h1 className="text-3xl font-bold">Not found</h1>
        <p className="text-slate-600">This project does not exist.</p>
      </main>
    );
  }

  return (
    <main className="mx-auto max-w-2xl px-6 py-12">
      <Link to="/projects" className="mb-6 inline-block font-semibold text-brand focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-brand">
        Back to projects
      </Link>
      <img
        src={p.img}
        alt=""
        style={{ viewTransitionName: `thumb-${id}` }}
        className="mb-6 h-72 w-full rounded-3xl bg-slate-200 object-cover"
      />
      <h1 className="mb-4 text-4xl font-bold tracking-tight">{p.title}</h1>
      <p className="leading-relaxed text-slate-600">{p.summary}</p>
    </main>
  );
}
```

## Rules for implementation
- Wrap navigation in `startViewTransition` only when supported and when reduced motion is not requested.
- Keep `viewTransitionName` values unique per element on the page.
- Move focus to the page heading on every route change.
- Preserve modifier-key and middle-click behaviour by letting the browser handle those clicks.
