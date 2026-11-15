---
name: scroll-reveal-code
description: Production-ready HTML, CSS and vanilla JavaScript scroll reveal using IntersectionObserver, staggered children, a no-JavaScript fallback, reduced-motion support and one-time reveal. Use when implementing scroll-triggered entrance animations in plain HTML, CSS and JavaScript.
---

# Scroll Reveal: HTML, CSS and JavaScript Implementation

```html
<!doctype html>
<html lang="en" class="js">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Scroll Reveal</title>
<script>document.documentElement.classList.add("js");</script>
<style>
  :root { --ink: #111418; --muted: #5b6470; --accent: #3b5bdb; --ease: cubic-bezier(0.2, 0.6, 0.2, 1); }
  * { box-sizing: border-box; }
  body { margin: 0; font-family: system-ui, sans-serif; color: var(--ink); background: #fff; }

  .section { min-height: 60vh; padding: 96px 24px; max-width: 960px; margin: 0 auto; }
  .section h2 { font-size: clamp(1.75rem, 4vw, 2.5rem); margin: 0 0 32px; letter-spacing: -0.02em; }

  .cards { display: grid; gap: 24px; grid-template-columns: repeat(auto-fit, minmax(240px, 1fr)); }
  .card { background: #f5f6f8; border-radius: 16px; padding: 28px; }
  .card h3 { margin: 0 0 8px; font-size: 1.125rem; }
  .card p { margin: 0; color: var(--muted); line-height: 1.6; }

  /* Hidden state only applies when JavaScript is available */
  .js .reveal {
    opacity: 0;
    transform: translate3d(0, 24px, 0);
    transition: opacity 500ms var(--ease), transform 500ms var(--ease);
    transition-delay: var(--delay, 0ms);
    will-change: opacity, transform;
  }
  .js .reveal.is-visible { opacity: 1; transform: none; }

  @media (prefers-reduced-motion: reduce) {
    .js .reveal { opacity: 1; transform: none; transition: none; }
  }
</style>
</head>
<body>
  <section class="section">
    <h2 class="reveal">Why teams choose us</h2>
    <div class="cards">
      <article class="card reveal" style="--delay:0ms">
        <h3>Clear onboarding</h3>
        <p>New members are productive within their first day.</p>
      </article>
      <article class="card reveal" style="--delay:100ms">
        <h3>Reliable delivery</h3>
        <p>Releases ship on schedule with fewer rollbacks.</p>
      </article>
      <article class="card reveal" style="--delay:200ms">
        <h3>Transparent pricing</h3>
        <p>Every cost is listed before you commit.</p>
      </article>
    </div>
  </section>

  <section class="section">
    <h2 class="reveal">Get started today</h2>
    <p class="reveal" style="margin:0">Create an account in under two minutes and invite your team.</p>
  </section>

<script>
  (function () {
    const reduce = window.matchMedia("(prefers-reduced-motion: reduce)").matches;
    const items = document.querySelectorAll(".reveal");
    if (!("IntersectionObserver" in window) || reduce) {
      items.forEach((el) => el.classList.add("is-visible"));
      return;
    }

    const observer = new IntersectionObserver(
      (entries) => {
        entries.forEach((entry) => {
          if (entry.isIntersecting) {
            entry.target.classList.add("is-visible");
            observer.unobserve(entry.target);
          }
        });
      },
      { threshold: 0.15, rootMargin: "0px 0px -8% 0px" }
    );

    items.forEach((el) => observer.observe(el));
  })();
</script>
</body>
</html>
```

## Rules for implementation
- Hide content only under the `.js` class so it stays visible without JavaScript.
- Use IntersectionObserver and unobserve after the reveal (one-time reveal).
- Keep the stagger in one `--delay` variable, capped near 400ms in total.
- Show everything immediately for reduced-motion users.
