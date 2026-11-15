---
name: tactile-feedback-code
description: Production-ready HTML, CSS and vanilla JavaScript tactile buttons with immediate press feedback, spring-like release, cancel-on-slide handling, optional Vibration API haptics and reduced-motion support. Use when implementing touch-responsive buttons and controls in plain HTML, CSS and JavaScript.
---

# Tactile Feedback: HTML, CSS and JavaScript Implementation

```html
<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Tactile Controls</title>
<style>
  :root {
    --ink: #111418;
    --accent: #3b5bdb;
    --accent-press: #2f4bc0;
    --surface: #ffffff;
    --spring: cubic-bezier(0.34, 1.56, 0.64, 1);
  }
  * { box-sizing: border-box; -webkit-tap-highlight-color: transparent; }
  body { margin: 0; min-height: 100vh; display: grid; place-items: center; font-family: system-ui, sans-serif; background: #f5f6f8; color: var(--ink); }

  .stack { display: grid; gap: 16px; width: min(360px, 92vw); }

  .tap {
    min-height: 52px;
    padding: 0 20px;
    border: 0;
    border-radius: 14px;
    background: var(--accent);
    color: #fff;
    font: inherit;
    font-weight: 600;
    cursor: pointer;
    user-select: none;
    touch-action: manipulation;
    transition: transform 160ms var(--spring), background-color 120ms ease;
    box-shadow: 0 4px 10px rgba(59, 91, 219, 0.25);
  }
  .tap:active, .tap.is-pressed { transform: scale(0.97); background: var(--accent-press); box-shadow: 0 2px 4px rgba(59, 91, 219, 0.2); }
  .tap:focus-visible { outline: 3px solid var(--ink); outline-offset: 3px; }
  .tap:disabled { opacity: 0.5; transform: none; cursor: not-allowed; }

  .status { min-height: 1.4em; margin: 0; color: #5b6470; text-align: center; font-size: 15px; }

  @media (prefers-reduced-motion: reduce) {
    .tap { transition: background-color 120ms ease; }
    .tap:active, .tap.is-pressed { transform: none; }
  }
</style>
</head>
<body>
  <main class="stack">
    <button class="tap" type="button" data-haptic="light">Select plan</button>
    <button class="tap" type="button" data-haptic="medium" id="save">Save changes</button>
    <p class="status" id="status" role="status" aria-live="polite"></p>
  </main>

<script>
  const status = document.getElementById("status");
  const canVibrate = "vibrate" in navigator;

  function haptic(kind) {
    if (!canVibrate) return;
    const patterns = { light: 8, medium: 18, success: [12, 40, 12] };
    navigator.vibrate(patterns[kind] ?? 8);
  }

  document.querySelectorAll(".tap").forEach((btn) => {
    let cancelled = false;

    // Pressed state follows the pointer, and cancels if the pointer slides away.
    btn.addEventListener("pointerdown", (e) => {
      cancelled = false;
      btn.classList.add("is-pressed");
      btn.setPointerCapture(e.pointerId);
    });
    btn.addEventListener("pointermove", (e) => {
      const r = btn.getBoundingClientRect();
      const inside = e.clientX >= r.left && e.clientX <= r.right && e.clientY >= r.top && e.clientY <= r.bottom;
      if (!inside) {
        cancelled = true;
        btn.classList.remove("is-pressed");
      }
    });
    const release = () => btn.classList.remove("is-pressed");
    btn.addEventListener("pointerup", (e) => {
      release();
      if (!cancelled && btn.dataset.haptic) haptic(btn.dataset.haptic);
    });
    btn.addEventListener("pointercancel", release);
    btn.addEventListener("pointerleave", release);
  });

  document.getElementById("save").addEventListener("click", async () => {
    status.textContent = "Saving...";
    await new Promise((r) => setTimeout(r, 900));
    haptic("success");
    status.textContent = "Changes saved.";
  });
</script>
</body>
</html>
```

## Rules for implementation
- Use `:active` and an `is-pressed` class for immediate press feedback.
- Cancel the action when the pointer slides off the control, as shown.
- Use `navigator.vibrate` only as an optional enhancement and never as the only signal.
- Remove scale motion under reduced motion; keep colour change.
