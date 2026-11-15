---
name: smart-form-validation-code
description: Production-ready HTML, CSS and vanilla JavaScript form validation with blur-time checks, live re-validation after an error, an error summary linked to fields, aria-describedby association, focus management on submit and a password strength hint. Use when implementing accessible, user-friendly form validation in plain HTML, CSS and JavaScript.
---

# Smart Form Validation: HTML, CSS and JavaScript Implementation

```html
<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Smart Validation</title>
<style>
  :root { --ink: #111418; --muted: #4a5160; --line: #8a93a3; --accent: #1f4fd1; --danger: #b42318; --ok: #067647; --surface: #fff; --bg: #f5f6f8; }
  * { box-sizing: border-box; }
  body { margin: 0; font-family: system-ui, sans-serif; color: var(--ink); background: var(--bg); line-height: 1.5; }
  :focus-visible { outline: 3px solid var(--accent); outline-offset: 3px; }

  form { max-width: 480px; margin: 40px auto; background: var(--surface); border: 1px solid #e4e7ec; border-radius: 16px; padding: 28px; display: grid; gap: 20px; }
  h1 { font-size: 1.5rem; margin: 0; }

  .summary { border: 2px solid var(--danger); background: #fef3f2; border-radius: 12px; padding: 14px 16px; color: #7a271a; }
  .summary[hidden] { display: none; }
  .summary h2 { font-size: 1rem; margin: 0 0 6px; }
  .summary ul { margin: 0; padding-left: 18px; }
  .summary a { color: #7a271a; font-weight: 600; }

  .field { display: grid; gap: 6px; }
  label { font-weight: 600; }
  .hint { color: var(--muted); font-size: 14px; margin: 0; }
  input {
    min-height: 44px; padding: 10px 12px; font: inherit; color: var(--ink);
    border: 1.5px solid var(--line); border-radius: 10px; background: #fff;
  }
  input[aria-invalid="true"] { border-color: var(--danger); background: #fffbfa; }
  input[aria-invalid="false"] { border-color: var(--ok); }
  .error { display: flex; gap: 8px; align-items: flex-start; color: var(--danger); font-size: 14px; margin: 0; }
  .error[hidden] { display: none; }
  .error::before { content: "!"; flex: 0 0 18px; height: 18px; border-radius: 50%; background: var(--danger); color: #fff; font-weight: 700; font-size: 12px; text-align: center; line-height: 18px; }

  button { min-height: 46px; border: 0; border-radius: 10px; background: var(--accent); color: #fff; font: inherit; font-weight: 600; cursor: pointer; }
  button:disabled { opacity: 0.6; cursor: wait; }
  .status { color: var(--ok); font-weight: 600; margin: 0; }
</style>
</head>
<body>
  <form id="signup" novalidate>
    <h1>Create your account</h1>

    <section class="summary" id="summary" tabindex="-1" aria-labelledby="summary-title" hidden>
      <h2 id="summary-title">Please fix the following</h2>
      <ul id="summary-list"></ul>
    </section>

    <div class="field">
      <label for="name">Full name</label>
      <input id="name" name="name" autocomplete="name" required aria-describedby="name-error">
      <p id="name-error" class="error" hidden></p>
    </div>

    <div class="field">
      <label for="email">Work email</label>
      <input id="email" name="email" type="email" autocomplete="email" required aria-describedby="email-hint email-error">
      <p id="email-hint" class="hint">We will send a confirmation to this address.</p>
      <p id="email-error" class="error" hidden></p>
    </div>

    <div class="field">
      <label for="password">Password</label>
      <input id="password" name="password" type="password" autocomplete="new-password" required aria-describedby="password-hint password-error">
      <p id="password-hint" class="hint">Use at least 8 characters, including a number.</p>
      <p id="password-error" class="error" hidden></p>
    </div>

    <button type="submit" id="submit">Create account</button>
    <p class="status" id="status" role="status" aria-live="polite"></p>
  </form>

<script>
  const rules = {
    name: {
      test: (v) => v.trim().length >= 2,
      message: "Enter your full name, at least two characters.",
    },
    email: {
      test: (v) => /^[^\s@]+@[^\s@]+\.[^\s@]{2,}$/.test(v.trim()),
      message: "Enter an email address in the format name@company.com.",
    },
    password: {
      test: (v) => v.length >= 8 && /\d/.test(v),
      message: "Use at least 8 characters and include one number.",
    },
  };

  const form = document.getElementById("signup");
  const summary = document.getElementById("summary");
  const summaryList = document.getElementById("summary-list");
  const status = document.getElementById("status");

  function fieldEl(name) { return form.elements.namedItem(name); }
  function errorEl(name) { return document.getElementById(`${name}-error`); }

  function validate(name, { showError }) {
    const input = fieldEl(name);
    const valid = rules[name].test(input.value);
    if (showError) {
      input.setAttribute("aria-invalid", String(!valid));
      const err = errorEl(name);
      err.hidden = valid;
      err.textContent = valid ? "" : rules[name].message;
    }
    return valid;
  }

  // Validate on blur, then re-validate live once an error is showing
  Object.keys(rules).forEach((name) => {
    const input = fieldEl(name);
    input.addEventListener("blur", () => validate(name, { showError: true }));
    input.addEventListener("input", () => {
      if (input.getAttribute("aria-invalid") === "true") validate(name, { showError: true });
    });
  });

  form.addEventListener("submit", (e) => {
    e.preventDefault();
    const invalid = Object.keys(rules).filter((name) => !validate(name, { showError: true }));

    if (invalid.length) {
      summaryList.innerHTML = "";
      invalid.forEach((name) => {
        const li = document.createElement("li");
        const a = document.createElement("a");
        a.href = `#${name}`;
        a.textContent = rules[name].message;
        a.addEventListener("click", (ev) => { ev.preventDefault(); fieldEl(name).focus(); });
        li.appendChild(a);
        summaryList.appendChild(li);
      });
      summary.hidden = false;
      summary.focus();
      status.textContent = "";
      return;
    }

    summary.hidden = true;
    const submit = document.getElementById("submit");
    submit.disabled = true;
    status.textContent = "Creating your account...";
    setTimeout(() => {
      submit.disabled = false;
      status.textContent = "Account created. Check your inbox to confirm.";
    }, 900);
  });
</script>
</body>
</html>
```

## Rules for implementation
- Validate on blur for format checks; re-validate live only after an error is shown.
- Associate every error with its field through `aria-describedby` and `aria-invalid`.
- On a failed submit, show a summary with links, and move focus to the summary.
- Keep user input intact whenever an error appears.
