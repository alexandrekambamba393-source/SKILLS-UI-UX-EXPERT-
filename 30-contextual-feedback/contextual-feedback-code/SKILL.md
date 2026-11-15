---
name: contextual-feedback-code
description: Production-ready HTML, CSS and vanilla JavaScript contextual feedback with a toast queue that pauses on hover and focus, an inline form error, a persistent warning banner with dismiss and typed success, error and warning messages with icons and accessible live regions. Use when implementing toasts, banners and inline feedback in plain HTML, CSS and JavaScript.
---

# Contextual Feedback: HTML, CSS and JavaScript Implementation

```html
<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Contextual Feedback</title>
<style>
  :root {
    --ink: #111418; --muted: #4a5160; --line: #d0d5dd; --accent: #1f4fd1;
    --ok: #067647; --ok-bg: #ecfdf3;
    --warn: #93370d; --warn-bg: #fffaeb;
    --err: #b42318; --err-bg: #fef3f2;
    --info: #1849a9; --info-bg: #eff8ff;
    --surface: #fff; --bg: #f5f6f8;
  }
  * { box-sizing: border-box; }
  body { margin: 0; font-family: system-ui, sans-serif; color: var(--ink); background: var(--bg); line-height: 1.5; }
  :focus-visible { outline: 3px solid var(--accent); outline-offset: 3px; }

  .page { max-width: 760px; margin: 40px auto; padding: 0 20px; display: grid; gap: 20px; }

  .banner { display: flex; gap: 12px; align-items: flex-start; padding: 14px 16px; border-radius: 12px; border: 1px solid var(--line); }
  .banner .icon { flex: 0 0 22px; height: 22px; border-radius: 50%; display: grid; place-items: center; font-weight: 700; font-size: 13px; color: #fff; }
  .banner .text { flex: 1; }
  .banner .text strong { display: block; margin-bottom: 2px; }
  .banner.warn { background: var(--warn-bg); color: var(--warn); border-color: #fedf89; }
  .banner.warn .icon { background: #dc6803; }
  .banner[hidden] { display: none; }
  .dismiss { min-width: 40px; min-height: 40px; border: 0; background: transparent; font: inherit; font-size: 20px; cursor: pointer; color: inherit; border-radius: 8px; }

  form.card { background: var(--surface); border: 1px solid #e4e7ec; border-radius: 16px; padding: 24px; display: grid; gap: 14px; }
  .field { display: grid; gap: 6px; }
  label { font-weight: 600; font-size: 14px; }
  input { min-height: 44px; padding: 0 12px; font: inherit; border: 1.5px solid #8a93a3; border-radius: 10px; }
  input[aria-invalid="true"] { border-color: var(--err); }
  .inline-error { display: flex; gap: 8px; align-items: flex-start; color: var(--err); font-size: 14px; margin: 0; }
  .inline-error[hidden] { display: none; }
  .inline-error::before { content: "!"; flex: 0 0 18px; height: 18px; border-radius: 50%; background: var(--err); color: #fff; font-weight: 700; font-size: 12px; text-align: center; line-height: 18px; }

  button.primary { min-height: 46px; border: 0; border-radius: 10px; background: var(--accent); color: #fff; font: inherit; font-weight: 600; cursor: pointer; }

  /* Toast stack */
  .toasts { position: fixed; right: 20px; bottom: 20px; display: grid; gap: 10px; width: min(360px, calc(100% - 40px)); z-index: 60; }
  .toast {
    background: var(--ink); color: #fff; border-radius: 12px; padding: 12px 14px;
    display: flex; gap: 12px; align-items: center; box-shadow: 0 12px 28px rgba(0,0,0,0.25);
    animation: rise 220ms ease-out;
  }
  .toast .icon { flex: 0 0 20px; height: 20px; border-radius: 50%; display: grid; place-items: center; font-size: 12px; font-weight: 700; }
  .toast.ok .icon { background: #12b76a; }
  .toast.err .icon { background: #f04438; }
  .toast span.msg { flex: 1; font-size: 15px; }
  .toast button { background: none; border: 0; color: #9ec1ff; font: inherit; font-weight: 700; cursor: pointer; min-height: 36px; padding: 0 6px; }
  .toast button:focus-visible { outline: 2px solid #9ec1ff; }

  @keyframes rise { from { transform: translateY(8px); opacity: 0; } to { transform: none; opacity: 1; } }
  @media (prefers-reduced-motion: reduce) { .toast { animation: none; } }
</style>
</head>
<body>
  <main class="page">
    <section class="banner warn" id="banner" role="status">
      <span class="icon" aria-hidden="true">!</span>
      <div class="text">
        <strong>Trial ends in 3 days</strong>
        <span>Choose a plan to keep your projects and team access.</span>
      </div>
      <button class="dismiss" type="button" id="dismiss" aria-label="Dismiss trial notice">×</button>
    </section>

    <form class="card" id="profile" novalidate>
      <div class="field">
        <label for="email">Email address</label>
        <input id="email" type="email" autocomplete="email" aria-describedby="email-error">
        <p id="email-error" class="inline-error" hidden></p>
      </div>
      <button class="primary" type="submit">Save profile</button>
    </form>
  </main>

  <div class="toasts" id="toasts" aria-live="off"></div>

<script>
  const toasts = document.getElementById("toasts");

  // Toast with type, optional undo, auto-dismiss that pauses on hover and focus.
  function toast({ type = "ok", message, undo, duration = 5000 }) {
    const el = document.createElement("div");
    el.className = `toast ${type === "err" ? "err" : "ok"}`;
    el.setAttribute("role", type === "err" ? "alert" : "status");
    el.innerHTML = `<span class="icon" aria-hidden="true">${type === "err" ? "!" : "✓"}</span><span class="msg"></span>`;
    el.querySelector(".msg").textContent = message;

    if (undo) {
      const btn = document.createElement("button");
      btn.type = "button";
      btn.textContent = undo.label;
      btn.addEventListener("click", () => { undo.action(); remove(); });
      el.appendChild(btn);
    }

    toasts.appendChild(el);

    let timer = null;
    const start = () => { timer = setTimeout(remove, duration); };
    const stop = () => clearTimeout(timer);
    el.addEventListener("mouseenter", stop);
    el.addEventListener("mouseleave", start);
    el.addEventListener("focusin", stop);
    el.addEventListener("focusout", start);
    start();

    function remove() {
      clearTimeout(timer);
      el.remove();
    }
    return remove;
  }

  // Persistent banner dismissal
  document.getElementById("dismiss").addEventListener("click", () => {
    document.getElementById("banner").hidden = true;
  });

  // Inline validation error with focus to the field
  const form = document.getElementById("profile");
  const email = document.getElementById("email");
  const emailError = document.getElementById("email-error");

  form.addEventListener("submit", (e) => {
    e.preventDefault();
    const valid = /^[^\s@]+@[^\s@]+\.[^\s@]{2,}$/.test(email.value.trim());
    if (!valid) {
      email.setAttribute("aria-invalid", "true");
      emailError.hidden = false;
      emailError.textContent = "Enter an email address in the format name@company.com.";
      email.focus();
      return;
    }
    email.removeAttribute("aria-invalid");
    emailError.hidden = true;
    toast({
      type: "ok",
      message: "Profile saved.",
      undo: { label: "Undo", action: () => toast({ type: "ok", message: "Changes reverted." }) },
    });
  });
</script>
</body>
</html>
```

## Rules for implementation
- Use `role="status"` for success and info, and `role="alert"` only for errors that need immediate attention.
- Pause toast auto-dismiss while the user hovers or focuses within it.
- Pair every message type with an icon and text, never colour alone.
- Keep errors that need action persistent until resolved.
