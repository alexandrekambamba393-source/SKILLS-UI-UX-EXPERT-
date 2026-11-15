---
name: micro-interactions-code-react-tailwind
description: Production-ready React and Tailwind CSS micro-interactions including a press-feedback button, an accessible like toggle with an animated icon, and an async submit button with loading and success states announced via aria-live. Use when building small responsive feedback animations in a React project.
---

# Micro-interactions: React and Tailwind Implementation

## Tailwind configuration (tailwind.config.js)
```js
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: ["./src/**/*.{js,jsx,ts,tsx}"],
  theme: {
    extend: {
      colors: {
        ink: "#111418",
        muted: "#5b6470",
        brand: { DEFAULT: "#3b5bdb", hover: "#2f4bc0" },
        danger: "#c92a2a",
        ok: "#2b8a3e",
      },
      transitionTimingFunction: {
        "out-soft": "cubic-bezier(0.2, 0.6, 0.2, 1)",
      },
    },
  },
  plugins: [],
};
```

## Like toggle (LikeButton.tsx)
```tsx
import { useState } from "react";

export function LikeButton() {
  const [liked, setLiked] = useState(false);

  return (
    <button
      type="button"
      aria-pressed={liked}
      onClick={() => setLiked((v) => !v)}
      className={[
        "inline-flex min-h-10 items-center gap-2 rounded-full border px-3.5 font-medium transition-colors duration-200",
        "focus-visible:outline-[3px] focus-visible:outline-offset-3 focus-visible:outline-brand",
        liked ? "border-danger text-danger" : "border-slate-300 text-muted hover:border-slate-400",
      ].join(" ")}
    >
      <svg
        viewBox="0 0 24 24"
        aria-hidden="true"
        className={`h-[18px] w-[18px] transition-transform duration-200 ease-out motion-reduce:transition-none ${
          liked ? "scale-110 fill-current" : "fill-none"
        }`}
        stroke="currentColor"
        strokeWidth={2}
      >
        <path d="M12 21s-7-4.35-9.33-8.5C1.1 9.1 2.7 5 6.5 5c2.1 0 3.4 1.2 4 2.2.6-1 1.9-2.2 4-2.2 3.8 0 5.4 4.1 3.8 7.5C19 16.65 12 21 12 21z" />
      </svg>
      {liked ? "Liked" : "Like"}
    </button>
  );
}
```

## Async submit button (PublishButton.tsx)
```tsx
import { useState } from "react";

export function PublishButton({ onPublish }: { onPublish: () => Promise<void> }) {
  const [state, setState] = useState<"idle" | "loading" | "ok" | "error">("idle");

  async function handleClick() {
    setState("loading");
    try {
      await onPublish();
      setState("ok");
    } catch {
      setState("error");
    }
  }

  const message = {
    idle: "",
    loading: "",
    ok: "Published successfully.",
    error: "Could not publish. Try again.",
  }[state];

  return (
    <div className="grid gap-3">
      <button
        type="button"
        onClick={handleClick}
        disabled={state === "loading"}
        aria-busy={state === "loading"}
        className="inline-flex min-h-11 items-center justify-center rounded-lg bg-brand px-5 font-semibold text-white transition-[transform,background-color] duration-150 hover:bg-brand-hover active:scale-[0.97] focus-visible:outline-[3px] focus-visible:outline-offset-3 focus-visible:outline-brand disabled:cursor-progress disabled:opacity-60 disabled:active:scale-100 motion-reduce:transition-none"
      >
        {state === "loading" ? (
          <>
            <span className="mr-2 inline-block h-3.5 w-3.5 animate-spin rounded-full border-2 border-white/40 border-t-white motion-reduce:animate-none" aria-hidden="true" />
            Publishing
          </>
        ) : (
          "Publish"
        )}
      </button>
      <p role="status" aria-live="polite" className={`min-h-6 text-sm ${state === "ok" ? "text-ok" : state === "error" ? "text-danger" : "text-muted"}`}>
        {message}
      </p>
    </div>
  );
}
```

## Rules for implementation
- Keep press feedback under 120ms and state changes under 200ms.
- Use `aria-pressed` for toggles and `aria-live` for asynchronous outcomes.
- Keep spinners decorative and state in text.
- Use `motion-reduce:` variants so reduced-motion users see instant state changes.
