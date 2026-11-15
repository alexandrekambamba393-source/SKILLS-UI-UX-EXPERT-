---
name: accessibility-wcag-code-react-tailwind
description: Production-ready React and Tailwind CSS accessible components for WCAG 2.2 AA, including a labelled form field with error association, an accessible modal dialog with focus return, a skip link and a screen-reader-only utility. Use when building accessible UI components in a React project.
---

# Accessibility (WCAG 2.2): React and Tailwind Implementation

## Form field with error association (FormField.tsx)
```tsx
import { useId, type InputHTMLAttributes } from "react";

interface FormFieldProps extends InputHTMLAttributes<HTMLInputElement> {
  label: string;
  hint?: string;
  error?: string;
  required?: boolean;
}

export function FormField({ label, hint, error, required, id, ...rest }: FormFieldProps) {
  const autoId = useId();
  const fieldId = id ?? autoId;
  const hintId = `${fieldId}-hint`;
  const errorId = `${fieldId}-error`;
  const describedBy = [hint ? hintId : null, error ? errorId : null].filter(Boolean).join(" ") || undefined;

  return (
    <div className="grid gap-1.5">
      <label htmlFor={fieldId} className="font-semibold text-ink">
        {label}
        {required && (
          <>
            <span aria-hidden="true"> *</span>
            <span className="sr-only"> (required)</span>
          </>
        )}
      </label>
      {hint && <p id={hintId} className="text-sm text-muted">{hint}</p>}
      <input
        id={fieldId}
        required={required}
        aria-invalid={error ? true : undefined}
        aria-describedby={describedBy}
        className="min-h-11 rounded-[10px] border-[1.5px] border-line bg-white px-3 text-ink aria-[invalid=true]:border-danger focus-visible:outline-[3px] focus-visible:outline-offset-3 focus-visible:outline-brand"
        {...rest}
      />
      {error && (
        <p id={errorId} className="text-sm text-danger">
          {error}
        </p>
      )}
    </div>
  );
}
```

## Accessible dialog (AccessibleDialog.tsx)
```tsx
import { useEffect, useRef, type ReactNode } from "react";

export function AccessibleDialog({
  open,
  onClose,
  title,
  children,
}: {
  open: boolean;
  onClose: () => void;
  title: string;
  children: ReactNode;
}) {
  const ref = useRef<HTMLDialogElement>(null);

  useEffect(() => {
    const dialog = ref.current;
    if (!dialog) return;
    if (open && !dialog.open) dialog.showModal();
    if (!open && dialog.open) dialog.close();
  }, [open]);

  return (
    <dialog
      ref={ref}
      aria-labelledby="dialog-title"
      onClose={onClose}
      className="w-[calc(100%-32px)] max-w-md rounded-2xl border-0 p-7 shadow-2xl backdrop:bg-ink/50"
    >
      <h2 id="dialog-title" className="mb-3 text-xl font-semibold">{title}</h2>
      <div className="text-body">{children}</div>
      <div className="mt-6 flex justify-end">
        <button
          type="button"
          onClick={onClose}
          className="min-h-11 rounded-lg border-[1.5px] border-line bg-white px-4 font-semibold focus-visible:outline-[3px] focus-visible:outline-offset-3 focus-visible:outline-brand"
        >
          Close
        </button>
      </div>
    </dialog>
  );
}
```

## Page usage (SignupPage.tsx)
```tsx
import { useState } from "react";
import { AccessibleDialog } from "./AccessibleDialog";
import { FormField } from "./FormField";

export default function SignupPage() {
  const [email, setEmail] = useState("");
  const [error, setError] = useState<string | undefined>();
  const [helpOpen, setHelpOpen] = useState(false);
  const [status, setStatus] = useState("");

  function submit(e: React.FormEvent) {
    e.preventDefault();
    if (!email.trim()) return setError("Enter your email address, for example name@example.com.");
    if (!/^\S+@\S+\.\S+$/.test(email)) return setError("This email address does not look right. Check for typos.");
    setError(undefined);
    setStatus("Account created. Check your inbox to confirm.");
  }

  return (
    <>
      <a href="#content" className="sr-only focus:not-sr-only focus:absolute focus:left-3 focus:top-3 focus:z-50 focus:rounded-lg focus:bg-ink focus:px-3 focus:py-2 focus:text-white">
        Skip to form
      </a>

      <main id="content" className="mx-auto my-10 max-w-md px-5">
        <section aria-labelledby="form-title" className="rounded-2xl border border-line bg-white p-7">
          <h1 id="form-title" className="mb-2 text-2xl font-bold">Create your account</h1>
          <p className="mb-5 text-body">Fields marked as required must be completed before you continue.</p>

          <form onSubmit={submit} noValidate className="grid gap-5">
            <FormField
              label="Email address"
              type="email"
              autoComplete="email"
              required
              hint="We will send a confirmation link."
              error={error}
              value={email}
              onChange={(e) => setEmail(e.target.value)}
            />
            <button type="submit" className="min-h-11 rounded-lg bg-brand px-5 font-semibold text-white focus-visible:outline-[3px] focus-visible:outline-offset-3 focus-visible:outline-ink">
              Create account
            </button>
          </form>

          <p role="status" aria-live="polite" className="sr-only">{status}</p>

          <button type="button" aria-haspopup="dialog" onClick={() => setHelpOpen(true)} className="mt-4 min-h-11 rounded-lg border-[1.5px] border-line bg-white px-4 font-semibold focus-visible:outline-[3px] focus-visible:outline-offset-3 focus-visible:outline-brand">
            Why do you need my email?
          </button>
        </section>
      </main>

      <AccessibleDialog open={helpOpen} onClose={() => setHelpOpen(false)} title="Why we ask for email">
        We use your email to confirm your account and to send important security notices.
      </AccessibleDialog>
    </>
  );
}
```

## Rules for implementation
- Generate ids with `useId` and link hints and errors through `aria-describedby`.
- Use the native `<dialog>` element for modal focus handling; keep the opener and dialog in sync.
- Keep all focus indicators visible and at least 3px thick.
- Provide the skip link as the first focusable element.
