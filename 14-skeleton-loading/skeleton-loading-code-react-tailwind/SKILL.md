---
name: skeleton-loading-code-react-tailwind
description: Production-ready React and Tailwind CSS skeleton components with a shimmer utility, a card skeleton matching a real layout, delayed display to avoid flashing, aria-busy handling and error fallback. Use when building loading placeholders for lists and cards in a React project.
---

# Skeleton Loading: React and Tailwind Implementation

## Tailwind configuration (tailwind.config.js)
```js
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: ["./src/**/*.{js,jsx,ts,tsx}"],
  theme: {
    extend: {
      backgroundImage: {
        shimmer: "linear-gradient(90deg, #e6e8ec 0%, #f3f4f6 50%, #e6e8ec 100%)",
      },
      keyframes: {
        shimmer: {
          "0%": { backgroundPosition: "100% 0" },
          "100%": { backgroundPosition: "-100% 0" },
        },
      },
      animation: {
        shimmer: "shimmer 1.6s ease-in-out infinite",
      },
    },
  },
  plugins: [],
};
```

## Skeleton primitives (Skeleton.tsx)
```tsx
export function SkeletonBlock({ className = "" }: { className?: string }) {
  return (
    <div
      aria-hidden="true"
      className={`rounded-lg bg-shimmer bg-[length:200%_100%] animate-shimmer motion-reduce:animate-none motion-reduce:bg-none motion-reduce:bg-slate-200 ${className}`}
    />
  );
}

export function ArticleCardSkeleton() {
  return (
    <div className="grid gap-3.5 rounded-2xl border border-slate-200 bg-white p-5">
      <div className="flex items-center gap-3">
        <SkeletonBlock className="h-11 w-11 rounded-full" />
        <SkeletonBlock className="h-3.5 w-2/5" />
      </div>
      <SkeletonBlock className="h-5 w-[70%]" />
      <SkeletonBlock className="h-3.5 w-[92%]" />
      <SkeletonBlock className="h-3.5 w-[70%]" />
      <SkeletonBlock className="h-44 rounded-xl" />
    </div>
  );
}
```

## Delayed loading hook (useDelayedFlag.ts)
```tsx
import { useEffect, useState } from "react";

// Returns true only if loading lasts longer than the delay, avoiding flashes on fast requests.
export function useDelayedFlag(active: boolean, delay = 200) {
  const [show, setShow] = useState(false);
  useEffect(() => {
    if (!active) {
      setShow(false);
      return;
    }
    const id = setTimeout(() => setShow(true), delay);
    return () => clearTimeout(id);
  }, [active, delay]);
  return show;
}
```

## Feed with loading and error states (ArticleFeed.tsx)
```tsx
import { ArticleCardSkeleton } from "./Skeleton";
import { useDelayedFlag } from "./useDelayedFlag";

interface Article { id: string; author: string; title: string; excerpt: string; }

export default function ArticleFeed({
  articles,
  loading,
  error,
  onRetry,
}: {
  articles?: Article[];
  loading: boolean;
  error?: boolean;
  onRetry: () => void;
}) {
  const showSkeleton = useDelayedFlag(loading);

  if (error) {
    return (
      <div role="alert" className="mx-auto max-w-xl rounded-xl border border-red-200 bg-red-50 p-5 text-red-800">
        <p className="mb-3">We could not load articles.</p>
        <button onClick={onRetry} className="min-h-10 rounded-lg bg-red-700 px-4 font-semibold text-white focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-red-700">
          Try again
        </button>
      </div>
    );
  }

  return (
    <section aria-busy={loading} aria-label="Articles" className="mx-auto grid max-w-2xl gap-5 px-5 py-12">
      <p role="status" className="sr-only">{loading ? "Loading articles" : `${articles?.length ?? 0} articles loaded`}</p>

      {loading && showSkeleton && (
        <>
          <ArticleCardSkeleton />
          <ArticleCardSkeleton />
        </>
      )}

      {!loading &&
        articles?.map((a) => (
          <article key={a.id} className="grid gap-2 rounded-2xl border border-slate-200 bg-white p-5 transition-opacity duration-200">
            <p className="text-sm text-slate-500">{a.author}</p>
            <h2 className="text-xl font-semibold">{a.title}</h2>
            <p className="leading-relaxed text-slate-600">{a.excerpt}</p>
          </article>
        ))}
    </section>
  );
}
```

## Rules for implementation
- Mirror the real layout in every skeleton; avoid generic blocks.
- Delay the skeleton (`useDelayedFlag`) so fast responses do not flash.
- Hide decorative blocks from assistive technology and announce loading through a status region.
- Always provide an error state with a retry action.
