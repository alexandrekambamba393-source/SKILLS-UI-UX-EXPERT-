---
name: micro-interactions-code
description: Production-ready HTML, CSS and vanilla JavaScript micro-interactions including a press-feedback button, an animated like toggle, a loading-to-success submit button and an aria-live status message, all with reduced-motion support. Use when implementing small responsive feedback animations in plain HTML, CSS and JavaScript.
---

# Micro-interactions: HTML, CSS and JavaScript Implementation

```html
<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Micro-interactions</title>
<style>
  :root {
    --ink: #111418;
    --muted: #5b6470;
    --accent: #3b5bdb;
    --accent-hover: #2f4bc0;
    --danger: #c92a2a;
    --ok: #2b8a3e;
    --surface: #ffffff;
    --line: #d9dde3;
    --ease-out: cubic-bezier(0.2, 0.6, 0.2, 1);
    --fast: 120ms;
    --base: 200ms;
  }

  * { box-sizing: border-box; }
  body { margin: 0; min-height: 100vh; display: grid; place-items: center; background: #f5f6f8; color: var(--ink); font-family: system-ui, sans-serif; }

  .card { background: var(--surface); border: 1px solid var(--line); border-radius: 16px; padding: 28px; width: min(380px, 92vw); display: grid; gap: 20px; }

  .btn {
    min-height: 44px; padding: 0 20px; border: 0; border-radius: 10px;
    background: var(--accent); color: #fff; font: inherit; font-weight: 600; cursor: pointer;
    transition: transform var(--fast) var(--ease-out), background-color var(--base) var(--ease-out);
  }
  .btn:hover { background: var(--accent-hover); }
  .btn:active { transform: scale(0.97); }
  .btn:focus-visible { outline: 3px solid var(--accent); outline-offset: 3px; }
  .btn:disabled { opacity: 0.6; cursor: progress; transform: none; }

  .like {
    display: inline-flex; align-items: center; gap: 8px; min-height: 40px; padding: 0 14px;
    border: 1px solid var(--line); border-radius: 999px; background: var(--surface); color: var(--muted);
    font: inherit; cursor: pointer; transition: color var(--base), border-color var(--base);
  }
  .like[aria-pressed="true"] { color: var(--danger); border-color: var(--danger); }
  .like svg { width: 18px; height: 18px; transition: transform 220ms var(--ease-out); }
  .like[aria-pressed="true"] svg { fill: currentColor; transform: scale(1.15); }
  .like:focus-visible { outline: 3px solid var(--accent); outline-offset: 3px; }

  .status { min-height: 1.4em; font-size: 0.9375rem; color: var(--muted); margin: 0; }
  .status.ok { color: var(--ok); }
  .status.err { color: var(--danger); }

  .spinner {
    width: 14px; height: 14px; margin-right: 8px; border-radius: 50%;
    border: 2px solid rgba(255, 255, 255, 0.4); border-top-color: #fff;
    display: inline-block; vertical-align: -2px; animation: spin 700ms linear infinite;
  }
  @keyframes spin { to { transform: rotate(360deg); } }

  @media (prefers-reduced-motion: reduce) {
    *, *::before, *::after { transition: none !important; animation: none !important; }
  }
</style>
</head>
<body>
  <section class="card" aria-label="Post actions">
    <p style="margin:0">Launch notes for version 3.0 are ready to review.</p>

    <button class="like" type="button" aria-pressed="false" id="like">
      <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" aria-hidden="true">
        <path d="M12 21s-7-4.35-9.33-8.5C1.1 9.1 2.7 5 6.5 5c2.1 0 3.4 1.2 4 2.2.6-1 1.9-2.2 4-2.2 3.8 0 5.4 4.1 3.8 7.5C19 16.65 12 21 12 21z"/>
      </svg>
      <span id="like-text">Like</span>
    </button>

    <button class="btn" id="submit" type="button">Publish</button>
    <p class="status" id="status" role="status" aria-live="polite"></p>
  </section>

<script>
  const like = document.getElementById("like");
  const likeText = document.getElementById("like-text");
  like.addEventListener("click", () => {
    const pressed = like.getAttribute("aria-pressed") !== "true";
    like.setAttribute("aria-pressed", String(pressed));
    likeText.textContent = pressed ? "Liked" : "Like";
  });

  const submit = document.getElementById("submit");
  const status = document.getElementById("status");
  submit.addEventListener("click", async () => {
    const original = submit.textContent;
    submit.disabled = true;
    submit.innerHTML = '<span class="spinner" aria-hidden="true"></span>Publishing';
    status.className = "status";
    status.textContent = "";

    try {
      await new Promise((resolve) => setTimeout(resolve, 1200));
      status.className = "status ok";
      status.textContent = "Published successfully.";
    } catch {
      status.className = "status err";
      status.textContent = "Could not publish. Try again.";
    } finally {
      submit.disabled = false;
      submit.textContent = original;
    }
  });
</script>
</body>
</html>
```

## Rules for implementation
- Keep feedback under 120ms for presses and 200ms for state changes.
- Use `aria-pressed` for toggles and `aria-live` for asynchronous outcomes.
- Keep the spinner decorative (`aria-hidden`) and communicate progress through text.
- Disable reduced-motion animation globally, as shown, while keeping state changes readable.
