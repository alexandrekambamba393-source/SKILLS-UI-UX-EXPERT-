---
name: frictionless-ux-code-react-tailwind
description: Production-ready React and Tailwind CSS frictionless checkout with autofill-friendly fields, a smart default delivery option, a live order total, an undo toast instead of confirmation and a one-tap reorder action. Use when building a low-effort, high-clarity purchase or task flow in a React project.
---

# Frictionless UX: React and Tailwind Implementation

## Undo toast hook (useUndoToast.ts)
```tsx
import { useCallback, useEffect, useRef, useState } from "react";

export function useUndoToast(duration = 6000) {
  const [message, setMessage] = useState<string | null>(null);
  const [canUndo, setCanUndo] = useState(false);
  const timer = useRef<ReturnType<typeof setTimeout>>();
  const undoRef = useRef<() => void>(() => {});

  const show = useCallback((text: string, onUndo?: () => void) => {
    clearTimeout(timer.current);
    setMessage(text);
    setCanUndo(Boolean(onUndo));
    undoRef.current = onUndo ?? (() => {});
    timer.current = setTimeout(() => setMessage(null), duration);
  }, [duration]);

  const undo = useCallback(() => {
    clearTimeout(timer.current);
    undoRef.current();
    setMessage(null);
  }, []);

  useEffect(() => () => clearTimeout(timer.current), []);

  return { message, canUndo, show, undo };
}
```

## Checkout (QuickCheckout.tsx)
```tsx
import { useMemo, useState } from "react";
import { useUndoToast } from "./useUndoToast";

const SUBTOTAL = 4800;
const delivery = {
  standard: { label: "Standard", note: "3 to 5 days, free", cost: 0 },
  express: { label: "Express", note: "Next day, KES 500", cost: 500 },
} as const;

type Method = keyof typeof delivery;

export default function QuickCheckout() {
  const [method, setMethod] = useState<Method>("standard");
  const [placing, setPlacing] = useState(false);
  const { message, canUndo, show, undo } = useUndoToast();

  const total = useMemo(() => SUBTOTAL + delivery[method].cost, [method]);
  const fmt = (n: number) => `KES ${n.toLocaleString("en-US")}`;

  function placeOrder(e: React.FormEvent) {
    e.preventDefault();
    setPlacing(true);
    setTimeout(() => {
      setPlacing(false);
      show("Order placed.", () => show("Order cancelled."));
    }, 700);
  }

  return (
    <div className="mx-auto grid max-w-4xl gap-5 p-5 font-sans text-ink md:grid-cols-[1.3fr_1fr]">
      <form onSubmit={placeOrder} className="grid content-start gap-4 rounded-2xl border border-slate-200 bg-white p-6" autoComplete="on">
        <h1 className="text-xl font-semibold">Shipping</h1>

        <label className="grid gap-1.5 text-sm font-semibold">
          Full name
          <input name="name" autoComplete="name" defaultValue="Amara Otieno" className="min-h-11 rounded-[10px] border-[1.5px] border-slate-500 bg-white px-3 font-normal focus-visible:outline-[3px] focus-visible:outline-offset-2 focus-visible:outline-brand" />
        </label>
        <label className="grid gap-1.5 text-sm font-semibold">
          Street address
          <input name="address" autoComplete="street-address" defaultValue="14 Kilimani Road" className="min-h-11 rounded-[10px] border-[1.5px] border-slate-500 bg-white px-3 font-normal focus-visible:outline-[3px] focus-visible:outline-offset-2 focus-visible:outline-brand" />
        </label>

        <fieldset className="grid gap-2.5">
          <legend className="mb-1.5 text-sm font-semibold">Delivery</legend>
          {(Object.keys(delivery) as Method[]).map((key) => (
            <label
              key={key}
              className={`flex min-h-[52px] cursor-pointer items-center gap-3 rounded-xl border-[1.5px] p-3.5 ${
                method === key ? "border-brand bg-[#f3f6ff]" : "border-slate-300"
              }`}
            >
              <input type="radio" name="delivery" value={key} checked={method === key} onChange={() => setMethod(key)} className="h-[18px] w-[18px]" />
              <span>
                {delivery[key].label}
                <small className="block text-body">{delivery[key].note}</small>
              </span>
            </label>
          ))}
        </fieldset>

        <div className="flex items-center justify-between gap-3 rounded-xl bg-slate-50 px-3.5 py-3 text-sm">
          <p className="m-0">Paying with Visa ending 4242</p>
          <button type="button" className="min-h-10 px-1.5 font-semibold text-brand">Change</button>
        </div>

        <button
          type="submit"
          disabled={placing}
          className="min-h-[52px] rounded-xl bg-brand font-bold text-white disabled:cursor-wait disabled:opacity-60 focus-visible:outline-[3px] focus-visible:outline-offset-3 focus-visible:outline-ink"
        >
          {placing ? "Placing order..." : "Place order"}
        </button>
      </form>

      <aside aria-labelledby="sum-title" className="grid content-start gap-4 rounded-2xl border border-slate-200 bg-white p-6">
        <h2 id="sum-title" className="text-base font-semibold text-body">Order summary</h2>
        <dl className="grid grid-cols-[1fr_auto] gap-x-3 gap-y-2">
          <dt className="text-body">Items (2)</dt>
          <dd className="m-0 text-right font-semibold">{fmt(SUBTOTAL)}</dd>
          <dt className="text-body">Delivery</dt>
          <dd className="m-0 text-right font-semibold">{delivery[method].cost === 0 ? "Free" : fmt(delivery[method].cost)}</dd>
          <dt className="border-t border-slate-200 pt-2.5 text-body">Total</dt>
          <dd className="m-0 border-t border-slate-200 pt-2.5 text-right font-semibold">{fmt(total)}</dd>
        </dl>
        <button type="button" onClick={() => show("Last order added to your cart.")} className="min-h-11 rounded-xl border-[1.5px] border-slate-300 bg-white font-semibold focus-visible:outline-[3px] focus-visible:outline-offset-2 focus-visible:outline-brand">
          Reorder last purchase
        </button>
      </aside>

      {message && (
        <div role="status" aria-live="polite" className="fixed bottom-6 left-1/2 flex -translate-x-1/2 items-center gap-4 rounded-xl bg-ink px-4 py-3 text-white shadow-2xl">
          <span>{message}</span>
          {canUndo && (
            <button type="button" onClick={undo} className="min-h-10 px-2 font-bold text-[#9ec1ff] focus-visible:outline-2 focus-visible:outline-[#9ec1ff]">
              Undo
            </button>
          )}
        </div>
      )}
    </div>
  );
}
```

## Rules for implementation
- Use `autoComplete` values so autofill works, and keep field labels visible.
- Pre-select the common delivery option and update totals immediately.
- Use an undo toast for reversible actions instead of blocking confirmation.
- Keep one primary action per screen.
