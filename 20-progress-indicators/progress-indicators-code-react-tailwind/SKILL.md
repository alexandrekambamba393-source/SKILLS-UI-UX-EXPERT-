---
name: progress-indicators-code-react-tailwind
description: Production-ready React and Tailwind CSS progress indicators including a determinate progress bar with ARIA, a step indicator with aria-current, a labelled spinner for longer waits and milestone announcements. Use when building upload, onboarding or processing progress UI in a React project.
---

# Progress Indicators: React and Tailwind Implementation

## Progress bar (ProgressBar.tsx)
```tsx
interface ProgressBarProps {
  value: number;
  label: string;
  valueText?: string;
}

export function ProgressBar({ value, label, valueText }: ProgressBarProps) {
  const pct = Math.min(100, Math.max(0, Math.round(value)));
  return (
    <div>
      <div className="mb-2 flex justify-between text-sm text-body">
        <span>{label}</span>
        <span>{valueText ?? `${pct}%`}</span>
      </div>
      <div
        role="progressbar"
        aria-label={label}
        aria-valuemin={0}
        aria-valuemax={100}
        aria-valuenow={pct}
        className="h-2.5 overflow-hidden rounded-full bg-slate-200"
      >
        <div
          className="h-full rounded-full bg-brand transition-[width] duration-300 ease-out motion-reduce:transition-none"
          style={{ width: `${pct}%` }}
        />
      </div>
    </div>
  );
}
```

## Step indicator (Steps.tsx)
```tsx
export function Steps({ items, current }: { items: string[]; current: number }) {
  return (
    <ol aria-label="Setup steps" className="grid grid-cols-3 gap-2">
      {items.map((label, i) => {
        const state = i < current ? "done" : i === current ? "current" : "todo";
        return (
          <li
            key={label}
            aria-current={state === "current" ? "step" : undefined}
            className={[
              "border-t-[3px] pt-2.5 text-sm",
              state === "done" ? "border-ok text-ok" : "",
              state === "current" ? "border-brand font-semibold text-ink" : "",
              state === "todo" ? "border-slate-200 text-body" : "",
            ].join(" ")}
          >
            {state === "done" ? `${label} (done)` : label}
          </li>
        );
      })}
    </ol>
  );
}
```

## Upload panel (UploadPanel.tsx)
```tsx
import { useEffect, useState } from "react";
import { ProgressBar } from "./ProgressBar";

export default function UploadPanel() {
  const [pct, setPct] = useState(0);
  const [running, setRunning] = useState(false);
  const [announce, setAnnounce] = useState("");

  useEffect(() => {
    if (!running) return;
    const id = setInterval(() => {
      setPct((p) => {
        const next = Math.min(100, p + 25);
        if (next === 50 || next === 100) setAnnounce(`Upload ${next} percent complete.`);
        if (next === 100) {
          clearInterval(id);
          setRunning(false);
          setAnnounce("Upload complete.");
        }
        return next;
      });
    }, 900);
    return () => clearInterval(id);
  }, [running]);

  return (
    <section aria-labelledby="upload-title" className="mx-auto grid max-w-lg gap-6 rounded-2xl border border-slate-200 bg-white p-7 font-sans text-ink">
      <h2 id="upload-title" className="text-xl font-semibold">Uploading report</h2>
      <ProgressBar value={pct} label="report-q3.pdf" />

      {running && (
        <div className="flex items-center gap-3 text-sm text-body">
          <span className="h-[18px] w-[18px] animate-spin rounded-full border-[2.5px] border-slate-300 border-t-brand motion-reduce:animate-none" aria-hidden="true" />
          Processing your report, this can take a minute.
        </div>
      )}

      <button
        type="button"
        disabled={running}
        onClick={() => {
          setPct(0);
          setRunning(true);
        }}
        className="w-fit min-h-11 rounded-lg bg-brand px-5 font-semibold text-white disabled:cursor-progress disabled:opacity-50 focus-visible:outline-[3px] focus-visible:outline-offset-3 focus-visible:outline-ink"
      >
        {running ? "Uploading..." : "Start upload"}
      </button>

      <p role="status" aria-live="polite" className="sr-only">{announce}</p>
    </section>
  );
}
```

## Rules for implementation
- Use `role="progressbar"` with `aria-valuenow`, `aria-valuemin`, `aria-valuemax` and an accessible name.
- Drive progress from real operation state; never fake a percentage.
- Announce milestones only through a live region.
- Mark the current step with `aria-current="step"`.
