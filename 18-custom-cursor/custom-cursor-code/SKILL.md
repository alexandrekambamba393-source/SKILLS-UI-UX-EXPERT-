---
name: custom-cursor-code
description: Production-ready HTML, CSS and vanilla JavaScript custom cursor with a lerp-smoothed follower, state changes for links and buttons, a drag label, a fine-pointer-only media guard, reduced-motion support and pointer-events safety. Use when implementing an enhanced pointer on a desktop website in plain HTML, CSS and JavaScript.
---

# Custom Cursor: HTML, CSS and JavaScript Implementation

```html
<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Custom Cursor</title>
<style>
  :root { --ink: #111418; --accent: #3b5bdb; --paper: #f5f2ec; }
  * { box-sizing: border-box; }
  body { margin: 0; font-family: system-ui, sans-serif; color: var(--ink); background: var(--paper); min-height: 200vh; }

  .stage { max-width: 960px; margin: 0 auto; padding: 120px 24px; display: grid; gap: 40px; }
  .stage h1 { font-size: clamp(2.5rem, 7vw, 4.5rem); letter-spacing: -0.03em; margin: 0; }
  .stage a.work { color: var(--ink); font-size: 1.5rem; text-decoration: none; border-bottom: 2px solid var(--ink); align-self: start; }
  .stage a.work:focus-visible, .stage button { outline: 3px solid var(--accent); outline-offset: 4px; }
  .stage button { min-height: 44px; padding: 0 20px; border: 2px solid var(--ink); background: transparent; font: inherit; font-weight: 600; cursor: pointer; border-radius: 999px; align-self: start; }

  /* Only on fine pointers (mouse or trackpad) */
  @media (pointer: fine) {
    html.has-custom-cursor, html.has-custom-cursor * { cursor: none !important; }
    html.has-custom-cursor input, html.has-custom-cursor textarea, html.has-custom-cursor [contenteditable] { cursor: text !important; }
  }

  .cursor {
    position: fixed;
    top: 0;
    left: 0;
    width: 14px;
    height: 14px;
    margin: -7px 0 0 -7px;
    border-radius: 50%;
    background: var(--ink);
    pointer-events: none;
    z-index: 9999;
    opacity: 0;
    transition: width 200ms ease, height 200ms ease, margin 200ms ease, background-color 200ms ease, opacity 200ms ease;
    mix-blend-mode: difference;
  }
  .cursor.is-visible { opacity: 1; }
  .cursor.is-hover { width: 56px; height: 56px; margin: -28px 0 0 -28px; background: transparent; border: 2px solid var(--ink); }
  .cursor.is-drag { width: 84px; height: 84px; margin: -42px 0 0 -42px; background: var(--accent); color: #fff; font-size: 13px; font-weight: 700; display: grid; place-items: center; mix-blend-mode: normal; }

  @media (prefers-reduced-motion: reduce) {
    .cursor { transition: none; }
  }
</style>
</head>
<body>
  <main class="stage">
    <h1>Selected work</h1>
    <a class="work" href="#project-one" data-cursor="hover">Project One: Brand identity</a>
    <a class="work" href="#project-two" data-cursor="hover">Project Two: Campaign site</a>
    <div class="gallery" data-cursor="drag" tabindex="0" aria-label="Image gallery, drag to explore" style="height:220px;background:#e6e1d6;border-radius:16px"></div>
    <button type="button" data-cursor="hover">Start a project</button>
    <input type="text" placeholder="Your email" aria-label="Email address" style="padding:12px 14px;font:inherit;border:1px solid #bbb;border-radius:10px">
  </main>

  <div class="cursor" id="cursor" aria-hidden="true"></div>

<script>
  (function () {
    const fine = window.matchMedia("(pointer: fine)").matches;
    const reduce = window.matchMedia("(prefers-reduced-motion: reduce)").matches;
    if (!fine) return;

    const cursor = document.getElementById("cursor");
    document.documentElement.classList.add("has-custom-cursor");

    let x = 0, y = 0;      // target position (true pointer)
    let cx = 0, cy = 0;    // rendered position (smoothed)
    let visible = false;

    window.addEventListener("pointermove", (e) => {
      x = e.clientX;
      y = e.clientY;
      if (!visible) {
        visible = true;
        cx = x; cy = y;
        cursor.classList.add("is-visible");
      }
    }, { passive: true });

    document.addEventListener("pointerleave", () => {
      visible = false;
      cursor.classList.remove("is-visible");
    });

    const ease = reduce ? 1 : 0.2;
    function frame() {
      cx += (x - cx) * ease;
      cy += (y - cy) * ease;
      cursor.style.transform = `translate3d(${cx}px, ${cy}px, 0)`;
      requestAnimationFrame(frame);
    }
    requestAnimationFrame(frame);

    // State changes follow data attributes on interactive targets.
    document.addEventListener("pointerover", (e) => {
      const target = e.target.closest("[data-cursor]");
      cursor.classList.toggle("is-hover", target?.dataset.cursor === "hover");
      cursor.classList.toggle("is-drag", target?.dataset.cursor === "drag");
      cursor.textContent = target?.dataset.cursor === "drag" ? "Drag" : "";
    });
    document.addEventListener("pointerout", (e) => {
      if (!e.relatedTarget || !e.relatedTarget.closest?.("[data-cursor]")) {
        cursor.classList.remove("is-hover", "is-drag");
        cursor.textContent = "";
      }
    });
  })();
</script>
</body>
</html>
```

## Rules for implementation
- Restrict all custom cursor code to `(pointer: fine)` devices.
- Keep `pointer-events: none` on the cursor element so it never blocks clicks.
- Keep the native I-beam on text inputs and editable content.
- Remove the smoothing (instant follow) under `prefers-reduced-motion`.
