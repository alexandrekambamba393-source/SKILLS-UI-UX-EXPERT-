---
name: parallax-scrolling-code
description: Production-ready HTML, CSS and vanilla JavaScript parallax scrolling with a scroll-driven CSS fallback, rAF-throttled transforms, layered depth speeds and full reduced-motion and mobile safeguards. Use when implementing layered scroll-based depth effects in plain HTML, CSS and JavaScript.
---

# Parallax Scrolling: HTML, CSS and JavaScript Implementation

```html
<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Parallax Story</title>
<style>
  :root { --ink: #101418; --paper: #f5f2ec; --accent: #d9480f; }
  * { box-sizing: border-box; }
  body { margin: 0; font-family: system-ui, sans-serif; color: var(--ink); background: var(--paper); }

  .hero {
    position: relative;
    height: 100vh;
    overflow: hidden;
    display: grid;
    place-items: center;
    background: #1d2a3a;
    color: #fff;
    isolation: isolate;
  }

  .layer {
    position: absolute;
    inset: -15% -5%;
    will-change: transform;
    pointer-events: none;
    z-index: -1;
  }
  .layer--far { background: radial-gradient(circle at 30% 30%, #3a5a80, transparent 55%); }
  .layer--mid { background: radial-gradient(circle at 75% 60%, #d9480f55, transparent 50%); }
  .layer--near { background: radial-gradient(circle at 50% 80%, #ffffff22, transparent 40%); }

  .hero-content { max-width: 720px; padding: 24px; text-align: center; }
  .hero-content h1 { margin: 0 0 16px; font-size: clamp(2.5rem, 7vw, 4.5rem); line-height: 1.05; letter-spacing: -0.02em; }
  .hero-content p { margin: 0; font-size: 1.125rem; line-height: 1.6; color: rgba(255, 255, 255, 0.85); }

  .section { padding: 120px 24px; max-width: 680px; margin: 0 auto; }
  .section h2 { font-size: 2rem; margin: 0 0 12px; }
  .section p { line-height: 1.7; margin: 0; }

  @media (max-width: 768px) { .layer { transform: none !important; } }
  @media (prefers-reduced-motion: reduce) {
    .layer { transform: none !important; }
  }
  @supports (animation-timeline: scroll()) {
    @media (prefers-reduced-motion: no-preference) and (min-width: 769px) {
      .layer--far { animation: drift-far linear both; animation-timeline: scroll(root); animation-range: 0 100vh; }
      .layer--mid { animation: drift-mid linear both; animation-timeline: scroll(root); animation-range: 0 100vh; }
      .layer--near { animation: drift-near linear both; animation-timeline: scroll(root); animation-range: 0 100vh; }
    }
  }
  @keyframes drift-far { from { transform: translate3d(0, 0, 0); } to { transform: translate3d(0, 20vh, 0); } }
  @keyframes drift-mid { from { transform: translate3d(0, 0, 0); } to { transform: translate3d(0, 40vh, 0); } }
  @keyframes drift-near { from { transform: translate3d(0, 0, 0); } to { transform: translate3d(0, -12vh, 0); } }
</style>
</head>
<body>
  <header class="hero" id="hero">
    <div class="layer layer--far" data-speed="0.2" aria-hidden="true"></div>
    <div class="layer layer--mid" data-speed="0.4" aria-hidden="true"></div>
    <div class="layer layer--near" data-speed="-0.15" aria-hidden="true"></div>
    <div class="hero-content">
      <h1>Stories that unfold with depth</h1>
      <p>Scroll to see how layers move at different speeds to create a sense of space.</p>
    </div>
  </header>

  <section class="section">
    <h2>Built to stay readable</h2>
    <p>Text never moves with the background. The content plane stays fixed while decorative layers drift behind it.</p>
  </section>

<script>
  (function () {
    const reduce = window.matchMedia("(prefers-reduced-motion: reduce)");
    const narrow = window.matchMedia("(max-width: 768px)");
    // Fallback for browsers without scroll-driven animations.
    if (CSS.supports("animation-timeline: scroll()")) return;

    const layers = Array.from(document.querySelectorAll("[data-speed]"));
    let ticking = false;

    function update() {
      ticking = false;
      if (reduce.matches || narrow.matches) {
        layers.forEach((el) => (el.style.transform = ""));
        return;
      }
      const y = window.scrollY;
      layers.forEach((el) => {
        const speed = parseFloat(el.dataset.speed);
        el.style.transform = `translate3d(0, ${(y * speed).toFixed(2)}px, 0)`;
      });
    }

    window.addEventListener("scroll", () => {
      if (!ticking) {
        requestAnimationFrame(update);
        ticking = true;
      }
    }, { passive: true });

    reduce.addEventListener("change", update);
    narrow.addEventListener("change", update);
    update();
  })();
</script>
</body>
</html>
```

## Rules for implementation
- Use `transform` only; never animate `top`, `margin` or layout properties during scroll.
- Throttle scroll work with `requestAnimationFrame` and use passive listeners.
- Disable motion for reduced-motion users and below 768px, as shown.
- Keep text in the non-moving content plane.
