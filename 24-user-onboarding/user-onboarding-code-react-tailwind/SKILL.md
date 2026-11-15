---
name: user-onboarding-code-react-tailwind
description: Production-ready React and Tailwind CSS onboarding with a setup checklist driven by product events, a skippable welcome tour with keyboard handling, an actionable empty state and persisted progress. Use when building a first-run experience and setup checklist in a React project.
---

# User Onboarding: React and Tailwind Implementation

## Onboarding state hook (useOnboarding.ts)
```tsx
import { useCallback, useEffect, useState } from "react";

type StepKey = "profile" | "project" | "invite";
type OnboardingState = Partial<Record<StepKey, boolean>> & { tourSeen?: boolean };

const KEY = "onboarding-v1";

function read(): OnboardingState {
  try {
    return JSON.parse(localStorage.getItem(KEY) ?? "{}");
  } catch {
    return {};
  }
}

export function useOnboarding() {
  const [state, setState] = useState<OnboardingState>({});

  useEffect(() => {
    setState(read());
  }, []);

  const complete = useCallback((key: StepKey) => {
    setState((prev) => {
      if (prev[key]) return prev;
      const next = { ...prev, [key]: true };
      try {
        localStorage.setItem(KEY, JSON.stringify(next));
      } catch {
        /* storage unavailable */
      }
      return next;
    });
  }, []);

  const dismissTour = useCallback(() => {
    setState((prev) => {
      const next = { ...prev, tourSeen: true };
      try {
        localStorage.setItem(KEY, JSON.stringify(next));
      } catch {
        /* ignore */
      }
      return next;
    });
  }, []);

  return { state, complete, dismissTour };
}
```

## Checklist (SetupChecklist.tsx)
```tsx
type StepKey = "profile" | "project" | "invite";

const steps: { key: StepKey; label: string }[] = [
  { key: "profile", label: "Complete your profile" },
  { key: "project", label: "Create your first project" },
  { key: "invite", label: "Invite a teammate" },
];

export function SetupChecklist({
  state,
  onStart,
}: {
  state: Partial<Record<StepKey, boolean>>;
  onStart: (key: StepKey) => void;
}) {
  const done = steps.filter((s) => state[s.key]).length;

  return (
    <section aria-labelledby="setup-title" className="rounded-2xl border border-line bg-white p-6">
      <h2 id="setup-title" className="text-xl font-semibold">Set up your workspace</h2>
      <p role="status" aria-live="polite" className="mb-4 text-sm text-body">
        {done} of {steps.length} complete
      </p>
      <ol className="grid gap-2.5">
        {steps.map((s) => {
          const isDone = !!state[s.key];
          return (
            <li
              key={s.key}
              className={`flex items-center justify-between gap-3 rounded-xl border px-4 py-3.5 ${
                isDone ? "border-[#b9e3c6] bg-[#f1faf3]" : "border-line"
              }`}
            >
              <span className="flex items-center gap-3 font-semibold">
                <span
                  aria-hidden="true"
                  className={`grid h-[22px] w-[22px] place-items-center rounded-full border-2 text-[13px] text-white ${
                    isDone ? "border-ok bg-ok" : "border-slate-300"
                  }`}
                >
                  {isDone ? "✓" : ""}
                </span>
                {s.label}
              </span>
              <button
                type="button"
                disabled={isDone}
                onClick={() => onStart(s.key)}
                className="min-h-10 rounded-lg border-[1.5px] border-brand bg-white px-3.5 font-semibold text-brand disabled:border-line disabled:text-body focus-visible:outline-[3px] focus-visible:outline-offset-3 focus-visible:outline-brand"
              >
                {isDone ? "Done" : "Start"}
              </button>
            </li>
          );
        })}
      </ol>
    </section>
  );
}
```

## Welcome tour (WelcomeTour.tsx)
```tsx
import { useEffect, useRef, useState } from "react";

const tour = [
  { title: "Projects", body: "Group related work in projects to keep teammates aligned." },
  { title: "Tasks", body: "Add tasks to a project and assign owners to track progress." },
  { title: "Invites", body: "Bring your team in once your first project is ready." },
];

export function WelcomeTour({ onFinish }: { onFinish: () => void }) {
  const [index, setIndex] = useState(0);
  const nextRef = useRef<HTMLButtonElement>(null);
  const isLast = index === tour.length - 1;

  useEffect(() => {
    nextRef.current?.focus();
    const onKey = (e: KeyboardEvent) => {
      if (e.key === "Escape") onFinish();
    };
    document.addEventListener("keydown", onKey);
    return () => document.removeEventListener("keydown", onKey);
  }, [onFinish]);

  const step = tour[index];

  return (
    <aside
      role="dialog"
      aria-labelledby="tour-title"
      aria-describedby="tour-body"
      className="fixed bottom-5 right-5 z-50 w-[min(340px,calc(100%-40px))] rounded-2xl bg-ink p-5 text-white shadow-2xl"
    >
      <h3 id="tour-title" className="mb-1.5 text-base font-semibold">{step.title}</h3>
      <p id="tour-body" className="mb-4 text-[15px] text-white/85">{step.body}</p>
      <div className="flex items-center justify-between gap-2">
        <span className="text-sm text-slate-400" aria-live="polite">
          {index + 1} of {tour.length}
        </span>
        <div className="flex gap-1.5">
          <button type="button" onClick={onFinish} className="min-h-10 rounded-lg px-3.5 font-semibold text-slate-300 hover:text-white focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-white">
            Skip tour
          </button>
          <button
            ref={nextRef}
            type="button"
            onClick={() => (isLast ? onFinish() : setIndex((i) => i + 1))}
            className="min-h-10 rounded-lg bg-white px-3.5 font-semibold text-ink focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-white"
          >
            {isLast ? "Finish" : "Next"}
          </button>
        </div>
      </div>
    </aside>
  );
}
```

## Page (OnboardingPage.tsx)
```tsx
import { SetupChecklist } from "./SetupChecklist";
import { WelcomeTour } from "./WelcomeTour";
import { useOnboarding } from "./useOnboarding";

export default function OnboardingPage() {
  const { state, complete, dismissTour } = useOnboarding();

  return (
    <main className="mx-auto grid max-w-3xl gap-6 px-5 py-10 font-sans text-ink">
      <SetupChecklist state={state} onStart={complete} />

      <section aria-labelledby="empty-title" className="rounded-2xl border border-dashed border-slate-300 bg-white px-6 py-10 text-center">
        <h3 id="empty-title" className="mb-2 text-xl font-semibold">No projects yet</h3>
        <p className="mx-auto mb-5 max-w-md text-body">
          Projects hold your files, tasks and team activity. Create one to see your dashboard come to life.
        </p>
        <button
          type="button"
          onClick={() => complete("project")}
          className="min-h-11 rounded-lg bg-brand px-5 font-semibold text-white focus-visible:outline-[3px] focus-visible:outline-offset-3 focus-visible:outline-ink"
        >
          Create a project
        </button>
      </section>

      {!state.tourSeen && <WelcomeTour onFinish={dismissTour} />}
    </main>
  );
}
```

## Rules for implementation
- Complete checklist items from real product events via `complete(key)`, not manual toggles.
- Persist tour and checklist state so the tour appears once.
- Keep the tour skippable and dismissible with Escape.
- Keep empty states actionable with a single clear primary button.
