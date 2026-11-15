---
name: empty-states-code-react-tailwind
description: Production-ready React and Tailwind CSS empty state components covering first use, no search results and error with retry, built from a single reusable EmptyState component with an accessible heading and primary action. Use when building empty, no-results and error screens in a React project.
---

# Empty States: React and Tailwind Implementation

## Reusable empty state (EmptyState.tsx)
```tsx
import type { ReactNode } from "react";

interface EmptyStateProps {
  icon?: ReactNode;
  title: string;
  description: string;
  primary?: { label: string; onClick: () => void };
  secondary?: { label: string; href: string };
  tone?: "neutral" | "error";
  id?: string;
}

export function EmptyState({ icon, title, description, primary, secondary, tone = "neutral", id }: EmptyStateProps) {
  const isError = tone === "error";
  return (
    <section
      role={isError ? "alert" : undefined}
      aria-labelledby={id}
      className={`grid justify-items-center gap-2.5 rounded-2xl border-[1.5px] border-dashed px-6 py-10 text-center ${
        isError ? "border-[#f1b8b3] bg-[#fffbfa]" : "border-slate-300 bg-white"
      }`}
    >
      {icon && (
        <div
          aria-hidden="true"
          className={`mb-1.5 grid h-14 w-14 place-items-center rounded-2xl ${
            isError ? "bg-[#fef3f2] text-danger" : "bg-[#eef2ff] text-brand"
          }`}
        >
          {icon}
        </div>
      )}
      <h3 id={id} className="text-xl font-semibold">{title}</h3>
      <p className="max-w-[46ch] text-body">{description}</p>
      {(primary || secondary) && (
        <div className="mt-2 flex flex-wrap justify-center gap-3">
          {primary && (
            <button
              type="button"
              onClick={primary.onClick}
              className="min-h-11 rounded-lg bg-brand px-[18px] font-semibold text-white focus-visible:outline-[3px] focus-visible:outline-offset-3 focus-visible:outline-brand"
            >
              {primary.label}
            </button>
          )}
          {secondary && (
            <a
              href={secondary.href}
              className="inline-flex min-h-11 items-center rounded-lg border-[1.5px] border-slate-300 bg-white px-[18px] font-semibold text-ink no-underline focus-visible:outline-[3px] focus-visible:outline-offset-3 focus-visible:outline-brand"
            >
              {secondary.label}
            </a>
          )}
        </div>
      )}
    </section>
  );
}
```

## Search with results status (ProjectSearch.tsx)
```tsx
import { useState } from "react";
import { EmptyState } from "./EmptyState";

const projects = ["Budget 2030", "Marketing plan", "Hiring pipeline"];

export default function ProjectSearch() {
  const [query, setQuery] = useState("budget 2031");
  const results = query.trim()
    ? projects.filter((p) => p.toLowerCase().includes(query.trim().toLowerCase()))
    : projects;

  return (
    <div className="mx-auto grid max-w-3xl gap-4 p-6 font-sans text-ink">
      <div className="flex flex-wrap gap-3">
        <label htmlFor="q" className="sr-only">Search projects</label>
        <input
          id="q"
          type="search"
          value={query}
          onChange={(e) => setQuery(e.target.value)}
          aria-describedby="results-status"
          className="min-h-11 flex-1 basis-60 rounded-lg border-[1.5px] border-slate-400 bg-white px-3 focus-visible:outline-[3px] focus-visible:outline-offset-2 focus-visible:outline-brand"
        />
        <button
          type="button"
          onClick={() => setQuery("")}
          className="min-h-11 rounded-lg border-[1.5px] border-slate-300 bg-white px-4 font-semibold focus-visible:outline-[3px] focus-visible:outline-offset-2 focus-visible:outline-brand"
        >
          Clear filters
        </button>
      </div>

      <p id="results-status" role="status" aria-live="polite" className="text-sm text-body">
        {results.length} {results.length === 1 ? "result" : "results"}
        {query.trim() ? ` for "${query.trim()}"` : ""}
      </p>

      {results.length === 0 ? (
        <EmptyState
          id="no-results-title"
          title={`No results for "${query.trim()}"`}
          description="Check the spelling, try a shorter term, or remove the year from your search."
          primary={{ label: "Clear search", onClick: () => setQuery("") }}
        />
      ) : (
        <ul className="grid gap-2">
          {results.map((r) => (
            <li key={r} className="rounded-xl border border-slate-200 bg-white px-4 py-3">{r}</li>
          ))}
        </ul>
      )}
    </div>
  );
}
```

## Error with retry (ReportsError.tsx)
```tsx
import { EmptyState } from "./EmptyState";

export function ReportsError({ onRetry }: { onRetry: () => void }) {
  return (
    <EmptyState
      id="reports-error-title"
      tone="error"
      title="We could not load your reports"
      description="This is usually temporary. Try again in a moment, or check your connection."
      primary={{ label: "Try again", onClick: onRetry }}
      secondary={{ label: "View service status", href: "/status" }}
    />
  );
}
```

## Rules for implementation
- Build all empty states from one component so headings, spacing and actions stay consistent.
- Use `role="alert"` only for errors; use a live region for result counts.
- Include exactly one primary action in each empty state.
- Keep icons decorative and never rely on them to convey meaning.
