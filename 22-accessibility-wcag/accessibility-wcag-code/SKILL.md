---
name: accessibility-wcag-code
description: Production-ready HTML, CSS and vanilla JavaScript accessible patterns for WCAG 2.2 AA, including a skip link, a labelled form with error association, a modal dialog with focus trap and return, visible focus styling, reduced-motion handling and a live region. Use when implementing accessible components in plain HTML, CSS and JavaScript.
---

# Accessibility (WCAG 2.2): HTML, CSS and JavaScript Implementation

```html
<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Accessible Patterns</title>
<style>
  :root {
    --ink: #111418;
    --muted: #4a5160;     /* 7.6:1 on white */
    --line: #8a93a3;      /* 3.3:1 on white, usable for control boundaries */
    --accent: #1f4fd1;    /* 6.1:1 on white */
    --danger: #b42318;    /* 6.6:1 on white */
    --surface: #ffffff;
    --focus: #1f4fd1;
  }
  * { box-sizing: border-box; }
  body { margin: 0; font-family: system-ui, sans-serif; color: var(--ink); background: #f5f6f8; line-height: 1.6; }

  .skip { position: absolute; left: 12px; top: -60px; background: var(--ink); color: #fff; padding: 10px 14px; border-radius: 8px; z-index: 100; }
  .skip:focus { top: 12px; }

  :focus-visible { outline: 3px solid var(--focus); outline-offset: 3px; }

  main { max-width: 560px; margin: 40px auto; padding: 0 20px; }
  .form-card { background: var(--surface); border: 1px solid var(--line); border-radius: 16px; padding: 28px; }
  h1 { font-size: 1.75rem; margin: 0 0 8px; }
  .hint { color: var(--muted); margin: 0 0 20px; }

  .field { display: grid; gap: 6px; margin-bottom: 18px; }
  label { font-weight: 600; }
  input {
    min-height: 44px; padding: 10px 12px; font: inherit; border: 1.5px solid var(--line);
    border-radius: 10px; background: #fff; color: var(--ink);
  }
  input[aria-invalid="true"] { border-color: var(--danger); }
  .error { color: var(--danger); font-size: 14px; margin: 0; }

  button.primary {
    min-height: 44px; padding: 0 20px; border: 0; border-radius: 10px; background: var(--accent); color: #fff; font: inherit; font-weight: 600; cursor: pointer;
  }

  /* Dialog */
  dialog { border: 0; border-radius: 16px; padding: 28px; max-width: 420px; width: calc(100% - 32px); box-shadow: 0 24px 60px rgba(0,0,0,0.25); }
  dialog::backdrop { background: rgba(17, 20, 24, 0.5); }
  .actions { display: flex; gap: 12px; justify-content: flex-end; margin-top: 20px; }
  .ghost { min-height: 44px; padding: 0 16px; border: 1.5px solid var(--line); border-radius: 10px; background: #fff; font: inherit; font-weight: 600; cursor: pointer; }

  .sr-only { position: absolute; width: 1px; height: 1px; overflow: hidden; clip: rect(0 0 0 0); white-space: nowrap; }

  @media (prefers-reduced-motion: reduce) { * { animation: none !important; transition: none !important; } }
</style>
</head>
<body>
  <a class="skip" href="#content">Skip to form</a>

  <main id="content">
    <section class="form-card" aria-labelledby="form-title">
      <h1 id="form-title">Create your account</h1>
      <p class="hint">Fields marked as required must be completed before you continue.</p>

      <form id="signup" novalidate>
        <div class="field">
          <label for="email">Email address <span aria-hidden="true">*</span><span class="sr-only"> (required)</span></label>
          <input id="email" name="email" type="email" autocomplete="email" required aria-describedby="email-hint email-error">
          <p id="email-hint" class="hint" style="margin:0;font-size:14px">We will send a confirmation link.</p>
          <p id="email-error" class="error" hidden></p>
        </div>

        <button class="primary" type="submit">Create account</button>
      </form>

      <p role="status" id="live" class="sr-only" aria-live="polite"></p>
      <button class="ghost" id="open-help" type="button" aria-haspopup="dialog" style="margin-top:16px">Why do you need my email?</button>
    </section>
  </main>

  <dialog id="help" aria-labelledby="help-title">
    <h2 id="help-title" style="margin-top:0;font-size:1.25rem">Why we ask for email</h2>
    <p>We use your email to confirm your account and to send important security notices.</p>
    <div class="actions">
      <button class="ghost" id="close-help" type="button">Close</button>
    </div>
  </dialog>

<script>
  // Form validation with programmatic error association
  const form = document.getElementById("signup");
  const email = document.getElementById("email");
  const error = document.getElementById("email-error");
  const live = document.getElementById("live");

  form.addEventListener("submit", (e) => {
    e.preventDefault();
    if (!email.value.trim()) {
      email.setAttribute("aria-invalid", "true");
      error.hidden = false;
      error.textContent = "Enter your email address, for example name@example.com.";
      email.focus();
      return;
    }
    if (!email.checkValidity()) {
      email.setAttribute("aria-invalid", "true");
      error.hidden = false;
      error.textContent = "This email address does not look right. Check for typos.";
      email.focus();
      return;
    }
    email.removeAttribute("aria-invalid");
    error.hidden = true;
    live.textContent = "Account created. Check your inbox to confirm.";
  });

  // Dialog with focus return
  const dialog = document.getElementById("help");
  const opener = document.getElementById("open-help");
  opener.addEventListener("click", () => dialog.showModal());
  document.getElementById("close-help").addEventListener("click", () => dialog.close());
  dialog.addEventListener("close", () => opener.focus());
</script>
</body>
</html>
```

## Rules for implementation
- Associate labels with inputs using `for` and `id`; never rely on placeholder text.
- Link hints and errors with `aria-describedby` and set `aria-invalid` on failure.
- Use the native `<dialog>` with `showModal()` for focus trap and Escape handling; return focus to the opener on close.
- Keep focus indicators visible and at least 3px thick.
