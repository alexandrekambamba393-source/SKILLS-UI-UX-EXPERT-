---
name: contextual-feedback-code-react-tailwind
description: Production-ready React and Tailwind CSS contextual feedback with a toast provider and hook that pauses on hover and focus, a persistent warning banner, an inline field error and typed success, error and warning messages with accessible live regions. Use when building toasts, banners and inline feedback in a React project.
---

# Contextual Feedback: React and Tailwind Implementation

## Toast provider (ToastProvider.tsx)
```tsx
import { createContext, useCallback, useContext, useRef, useState, type ReactNode } from "react";

type ToastType = "ok" | "err";
interface ToastItem {
  id: number;
  type: ToastType;
  message: string;
  undo?: { label: string; action: () => void };
}

interface ToastApi {
  show: (t: Omit<ToastItem, "id">, duration?: number) => void;
}

const ToastCtx = createContext<ToastApi | null>(null);

export function ToastProvider({ children }: { children: ReactNode }) {
  const [items, setItems] = useState<ToastItem[]>([]);
  const timers = useRef(new Map<number, ReturnType<typeof setTimeout>>());
  const seq = useRef(0);

  const remove = useCallback((id: number) => {
    clearTimeout(timers.current.get(id));
    timers.current.delete(id);
    setItems((prev) => prev.filter((t) => t.id !== id));
  }, []);

  const show = useCallback<ToastApi["show"]>((t, duration = 5000) => {
    const id = ++seq.current;
    setItems((prev) => [...prev, { ...t, id }]);
    timers.current.set(id, setTimeout(() => remove(id), duration));
  }, [remove]);

  return (
    <ToastCtx.Provider value={{ show }}>
      {children}
      <div className="fixed bottom-5 right-5 z-[60] grid w-[min(360px,calc(100%-40px))] gap-2.5">
        {items.map((t) => (
          <div
            key={t.id}
            role={t.type === "err" ? "alert" : "status"}
            onMouseEnter={() => clearTimeout(timers.current.get(t.id))}
            onMouseLeave={() => timers.current.set(t.id, setTimeout(() => remove(t.id), 3000))}
            onFocus={() => clearTimeout(timers.current.get(t.id))}
            onBlur={() => timers.current.set(t.id, setTimeout(() => remove(t.id), 3000))}
            className="flex items-center gap-3 rounded-xl bg-ink px-3.5 py-3 text-white shadow-2xl motion-reduce:animate-none"
          >
            <span
              aria-hidden="true"
              className={`grid h-5 w-5 shrink-0 place-items-center rounded-full text-xs font-bold ${
                t.type === "err" ? "bg-[#f04438]" : "bg-[#12b76a]"
              }`}
            >
              {t.type === "err" ? "!" : "✓"}
            </span>
            <span className="flex-1 text-[15px]">{t.message}</span>
            {t.undo && (
              <button
                type="button"
                onClick={() => {
                  t.undo!.action();
                  remove(t.id);
                }}
                className="min-h-9 px-1.5 font-bold text-[#9ec1ff] focus-visible:outline-2 focus-visible:outline-[#9ec1ff]"
              >
                {t.undo.label}
              </button>
            )}
          </div>
        ))}
      </div>
    </ToastCtx.Provider>
  );
}

export function useToast() {
  const ctx = useContext(ToastCtx);
  if (!ctx) throw new Error("useToast must be used inside ToastProvider");
  return ctx;
}
```

## Banner (WarningBanner.tsx)
```tsx
import { useState } from "react";

export function WarningBanner({ title, children }: { title: string; children: React.ReactNode }) {
  const [open, setOpen] = useState(true);
  if (!open) return null;

  return (
    <section role="status" className="flex items-start gap-3 rounded-xl border border-[#fedf89] bg-[#fffaeb] px-4 py-3.5 text-[#93370d]">
      <span aria-hidden="true" className="grid h-[22px] w-[22px] shrink-0 place-items-center rounded-full bg-[#dc6803] text-[13px] font-bold text-white">!</span>
      <div className="flex-1">
        <strong className="mb-0.5 block">{title}</strong>
        <span>{children}</span>
      </div>
      <button
        type="button"
        onClick={() => setOpen(false)}
        aria-label={`Dismiss: ${title}`}
        className="grid min-h-10 min-w-10 place-items-center rounded-lg text-xl hover:bg-black/5 focus-visible:outline-2 focus-visible:outline-[#93370d]"
      >
        ×
      </button>
    </section>
  );
}
```

## Form with inline error and toast (ProfileForm.tsx)
```tsx
import { useId, useRef, useState } from "react";
import { useToast } from "./ToastProvider";

export default function ProfileForm() {
  const { show } = useToast();
  const [email, setEmail] = useState("");
  const [error, setError] = useState<string | undefined>();
  const inputRef = useRef<HTMLInputElement>(null);
  const id = useId();

  function onSubmit(e: React.FormEvent) {
    e.preventDefault();
    if (!/^[^\s@]+@[^\s@]+\.[^\s@]{2,}$/.test(email.trim())) {
      setError("Enter an email address in the format name@company.com.");
      inputRef.current?.focus();
      return;
    }
    setError(undefined);
    show({
      type: "ok",
      message: "Profile saved.",
      undo: { label: "Undo", action: () => show({ type: "ok", message: "Changes reverted." }) },
    });
  }

  return (
    <form onSubmit={onSubmit} noValidate className="mx-auto grid max-w-xl gap-4 rounded-2xl border border-slate-200 bg-white p-6 font-sans text-ink">
      <div className="grid gap-1.5">
        <label htmlFor={`${id}-email`} className="text-sm font-semibold">Email address</label>
        <input
          id={`${id}-email`}
          ref={inputRef}
          type="email"
          autoComplete="email"
          value={email}
          onChange={(e) => setEmail(e.target.value)}
          aria-invalid={error ? true : undefined}
          aria-describedby={error ? `${id}-error` : undefined}
          className="min-h-11 rounded-[10px] border-[1.5px] border-slate-500 bg-white px-3 aria-[invalid=true]:border-danger focus-visible:outline-[3px] focus-visible:outline-offset-2 focus-visible:outline-brand"
        />
        {error && (
          <p id={`${id}-error`} className="flex items-start gap-2 text-sm text-danger">
            <span aria-hidden="true" className="grid h-[18px] w-[18px] shrink-0 place-items-center rounded-full bg-danger text-[12px] font-bold text-white">!</span>
            {error}
          </p>
        )}
      </div>
      <button type="submit" className="min-h-11 rounded-lg bg-brand font-semibold text-white focus-visible:outline-[3px] focus-visible:outline-offset-3 focus-visible:outline-ink">
        Save profile
      </button>
    </form>
  );
}
```

## Usage (App.tsx)
```tsx
import ProfileForm from "./ProfileForm";
import { ToastProvider } from "./ToastProvider";
import { WarningBanner } from "./WarningBanner";

export default function App() {
  return (
    <ToastProvider>
      <main className="mx-auto grid max-w-2xl gap-5 p-6 font-sans text-ink">
        <WarningBanner title="Trial ends in 3 days">
          Choose a plan to keep your projects and team access.
        </WarningBanner>
        <ProfileForm />
      </main>
    </ToastProvider>
  );
}
```

## Rules for implementation
- Use `role="status"` for success and info, and `role="alert"` for errors needing attention.
- Pause toast timers on hover and focus, and resume them when the user leaves.
- Pair each type with an icon and text.
- Keep banners dismissible and errors requiring action persistent until resolved.
