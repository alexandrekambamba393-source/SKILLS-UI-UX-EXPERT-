---
name: editorial-design-code
description: Production-ready HTML and CSS editorial article layout with an expressive serif headline, kicker, deck, drop cap, pull quote, full-bleed figure with caption and a readable measure. Use when implementing a magazine-style long-form article page in plain HTML and CSS.
---

# Editorial Design: HTML and CSS Implementation

```html
<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Editorial Article</title>
<style>
  :root {
    --paper: #faf8f4;
    --ink: #1a1a1a;
    --muted: #5c5a55;
    --rule: #d9d4c9;
    --accent: #a4312b;
    --serif: "Georgia", "Iowan Old Style", "Times New Roman", serif;
    --sans: "Inter", system-ui, sans-serif;
  }

  * { box-sizing: border-box; }
  body { margin: 0; background: var(--paper); color: var(--ink); font-family: var(--serif); }

  .article { max-width: 1120px; margin: 0 auto; padding: 96px 24px 120px; }

  .kicker {
    font-family: var(--sans);
    font-size: 0.8125rem;
    font-weight: 700;
    letter-spacing: 0.12em;
    text-transform: uppercase;
    color: var(--accent);
    margin: 0 0 16px;
  }

  h1 {
    font-size: clamp(2.75rem, 7vw, 5.5rem);
    line-height: 1;
    letter-spacing: -0.02em;
    font-weight: 700;
    margin: 0 0 24px;
    max-width: 16ch;
  }

  .deck {
    font-size: clamp(1.25rem, 2.4vw, 1.625rem);
    line-height: 1.45;
    color: var(--muted);
    max-width: 38ch;
    margin: 0 0 32px;
  }

  .byline {
    font-family: var(--sans);
    font-size: 0.875rem;
    color: var(--muted);
    padding-bottom: 24px;
    border-bottom: 1px solid var(--rule);
    margin-bottom: 64px;
  }

  .text {
    max-width: 66ch;
    margin: 0 auto;
    font-size: 1.1875rem;
    line-height: 1.7;
  }
  .text p { margin: 0 0 1.3em; }
  .text p:first-of-type::first-letter {
    float: left;
    font-size: 4.5em;
    line-height: 0.85;
    font-weight: 700;
    padding: 6px 12px 0 0;
    color: var(--accent);
  }

  .pullquote {
    max-width: 30ch;
    margin: 72px auto;
    padding: 24px 0;
    border-top: 2px solid var(--ink);
    border-bottom: 1px solid var(--rule);
    font-size: clamp(1.75rem, 3.6vw, 2.5rem);
    line-height: 1.25;
    font-style: italic;
    text-align: center;
  }

  figure { margin: 80px -12% 80px; }
  figure img { width: 100%; height: auto; display: block; aspect-ratio: 3 / 2; object-fit: cover; }
  figcaption {
    font-family: var(--sans);
    font-size: 0.875rem;
    color: var(--muted);
    margin-top: 12px;
    max-width: 66ch;
    margin-inline: auto;
  }

  a { color: var(--ink); text-decoration: underline; text-decoration-thickness: 1px; text-underline-offset: 3px; }
  a:hover { color: var(--accent); }
  a:focus-visible { outline: 2px solid var(--accent); outline-offset: 3px; }

  @media (max-width: 760px) {
    figure { margin: 56px 0; }
    .article { padding-top: 56px; }
  }
</style>
</head>
<body>
  <article class="article">
    <p class="kicker">Long read</p>
    <h1>The quiet return of the printed page</h1>
    <p class="deck">Readers are choosing slower formats, and designers are rediscovering what a well-set column can do.</p>
    <p class="byline">By Staff Writer · 12 min read</p>

    <div class="text">
      <p>Every good page begins with a decision about how long a line should be. For most readers, the answer is somewhere near seventy characters.</p>
      <p>The rest follows from that choice: the leading, the paragraph spacing, the rhythm that lets the eye move without effort.</p>
      <blockquote class="pullquote">Comfort is the first form of respect a page can offer.</blockquote>
      <p>Images are placed where they change the pace of the story, not where they simply fill space.</p>
    </div>

    <figure>
      <img src="https://images.unsplash.com/photo-1495020689067-958852a7765e?w=1600" alt="A stack of printed magazines on a wooden desk">
      <figcaption>Printed editions still set the standard for pacing and hierarchy.</figcaption>
    </figure>
  </article>
</body>
</html>
```

## Rules for implementation
- Keep the body column at about 66ch so the measure stays readable.
- Use a single accent colour for kickers, drop caps and pull quotes.
- Give every figure a caption and meaningful alt text; use empty alt for decorative images.
- Keep one `h1` and follow it with ordered `h2` and `h3` headings.
