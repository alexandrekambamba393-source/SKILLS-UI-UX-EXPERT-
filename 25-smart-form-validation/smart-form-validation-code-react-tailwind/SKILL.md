---
name: smart-form-validation-code-react-tailwind
description: Production-ready React and Tailwind CSS form validation with a reusable validation hook, blur-then-live validation, an accessible error summary and field-level error association. Use when building validated forms with accessible error handling in a React project.
---

# Smart Form Validation: React and Tailwind Implementation

## Validation hook (useFormValidation.ts)
```tsx
import { useRef, useState } from "react";

export type Rule<T> = { test: (value: T) => boolean; message: string };
export type Rules<V extends Record<string, string>> = { [K in keyof V]: Rule<string> };

export function useFormValidation<V extends Record<string, string>>(rules: Rules<V>) {
  const [values, setValues] = useState<V>({} as V);
  const [errors, setErrors] = useState<Partial<Record<keyof V, string>>>({});
  const [touched, setTouched] = useState<Partial<Record<keyof V, boolean>>>({});
  const summaryRef = useRef<HTMLDivElement>(null);

  function check(name: keyof V, value: string) {
    const ok = rules[name].test(value);
    setErrors((prev) => ({ ...prev, [name]: ok ? undefined : rules[name].message }));
    return ok;
  }

  function onChange(name: keyof V, value: string) {
    setValues((prev) => ({ ...prev, [name]: value }));
    // Live re-validation only after the field has shown an error
    if (touched[name] && errors[name]) check(name, value);
  }

  function onBlur(name: keyof V) {
    setTouched((prev) => ({ ...prev, [name]: true }));
    check(name, (values[name] ?? "") as string);
  }

  function submit(): boolean {
    const names = Object.keys(rules) as (keyof V)[];
    const failed = names.filter((n) => !check(n, (values[n] ?? "") as string));
    setTouched(Object.fromEntries(names.map((n) => [n, true])) as Partial<Record<keyof V, boolean>>);
    if (failed.length) {
      requestAnimationFrame(() => summaryRef.current?.focus());
      return false;
    }
    return true;
  }

  const failedNames = (Object.keys(errors) as (keyof V)[]).filter((n) => errors[n]);

  return { values, errors, failedNames, summaryRef, onChange, onBlur, submit };
}
```

## Field component (Field.tsx)
```tsx
import { useId, type InputHTMLAttributes } from "react";

interface FieldProps extends InputHTMLAttributes<HTMLInputElement> {
  label: string;
  hint?: string;
  error?: string;
}

export function Field({ label, hint, error, id, ...rest }: FieldProps) {
  const autoId = useId();
  const fid = id ?? autoId;
  const describedBy = [hint ? `${fid}-hint` : null, error ? `${fid}-error` : null].filter(Boolean).join(" ") || undefined;

  return (
    <div className="grid gap-1.5">
      <label htmlFor={fid} className="font-semibold">{label}</label>
      {hint && <p id={`${fid}-hint`} className="text-sm text-slate-600">{hint}</p>}
      <input
        id={fid}
        aria-invalid={error ? true : undefined}
        aria-describedby={describedBy}
        className="min-h-11 rounded-[10px] border-[1.5px] border-slate-500 bg-white px-3 text-ink aria-[invalid=true]:border-danger aria-[invalid=true]:bg-[#fffbfa] focus-visible:outline-[3px] focus-visible:outline-offset-3 focus-visible:outline-brand"
        {...rest}
      />
      {error && (
        <p id={`${fid}-error`} className="flex items-start gap-2 text-sm text-danger">
          <span aria-hidden="true" className="grid h-[18px] w-[18px] shrink-0 place-items-center rounded-full bg-danger text-[12px] font-bold text-white">!</span>
          {error}
        </p>
      )}
    </div>
  );
}
```

## Signup form (SignupForm.tsx)
```tsx
import { useState } from "react";
import { Field } from "./Field";
import { useFormValidation } from "./useFormValidation";

const rules = {
  name: { test: (v: string) => v.trim().length >= 2, message: "Enter your full name, at least two characters." },
  email: { test: (v: string) => /^[^\s@]+@[^\s@]+\.[^\s@]{2,}$/.test(v.trim()), message: "Enter an email address in the format name@company.com." },
  password: { test: (v: string) => v.length >= 8 && /\d/.test(v), message: "Use at least 8 characters and include one number." },
};

type Values = { name: string; email: string; password: string };

export default function SignupForm() {
  const { values, errors, failedNames, summaryRef, onChange, onBlur, submit } = useFormValidation<Values>(rules);
  const [status, setStatus] = useState("");

  function handleSubmit(e: React.FormEvent) {
    e.preventDefault();
    if (submit()) setStatus("Account created. Check your inbox to confirm.");
  }

  return (
    <form onSubmit={handleSubmit} noValidate className="mx-auto grid max-w-md gap-5 rounded-2xl border border-slate-200 bg-white p-7 font-sans text-ink">
      <h1 className="text-2xl font-bold">Create your account</h1>

      {failedNames.length > 0 && (
        <div ref={summaryRef} tabIndex={-1} role="alert" className="rounded-xl border-2 border-danger bg-[#fef3f2] p-4 text-[#7a271a]">
          <h2 className="mb-1.5 text-base font-semibold">Please fix the following</h2>
          <ul className="list-disc pl-5">
            {failedNames.map((n) => (
              <li key={n}>
                <a href={`#${n}`} className="font-semibold text-[#7a271a]">{errors[n]}</a>
              </li>
            ))}
          </ul>
        </div>
      )}

      <Field
        id="name"
        label="Full name"
        autoComplete="name"
        value={values.name ?? ""}
        error={errors.name}
        onChange={(e) => onChange("name", e.target.value)}
        onBlur={() => onBlur("name")}
      />
      <Field
        id="email"
        label="Work email"
        type="email"
        autoComplete="email"
        hint="We will send a confirmation to this address."
        value={values.email ?? ""}
        error={errors.email}
        onChange={(e) => onChange("email", e.target.value)}
        onBlur={() => onBlur("email")}
      />
      <Field
        id="password"
        label="Password"
        type="password"
        autoComplete="new-password"
        hint="Use at least 8 characters, including a number."
        value={values.password ?? ""}
        error={errors.password}
        onChange={(e) => onChange("password", e.target.value)}
        onBlur={() => onBlur("password")}
      />

      <button type="submit" className="min-h-12 rounded-lg bg-brand font-semibold text-white disabled:opacity-60 focus-visible:outline-[3px] focus-visible:outline-offset-3 focus-visible:outline-ink">
        Create account
      </button>
      <p role="status" aria-live="polite" className="font-semibold text-ok">{status}</p>
    </form>
  );
}
```

## Rules for implementation
- Validate on blur, and re-validate live only after an error is visible.
- Use `role="alert"` plus focus on the summary when submission fails.
- Link each error to its field with `aria-describedby` and `aria-invalid`.
- Keep entered values intact when errors appear.
