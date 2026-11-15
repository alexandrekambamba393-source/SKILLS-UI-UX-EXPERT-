---
name: editorial-design-code-react-tailwind
description: Production-ready React and Tailwind CSS editorial article components with a magazine headline, deck, byline, drop cap, pull quote and captioned full-bleed figure. Use when building a long-form article or publication page in a React project.
---

# Editorial Design: React and Tailwind Implementation

## Tailwind configuration (tailwind.config.js)
```js
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: ["./src/**/*.{js,jsx,ts,tsx}"],
  theme: {
    extend: {
      colors: {
        paper: "#faf8f4",
        ink: "#1a1a1a",
        muted: "#5c5a55",
        rule: "#d9d4c9",
        accent: "#a4312b",
      },
      fontFamily: {
        serif: ["Georgia", "Iowan Old Style", "Times New Roman", "serif"],
        sans: ["Inter", "system-ui", "sans-serif"],
      },
    },
  },
  plugins: [],
};
```

## Article components (Article.tsx)
```tsx
import type { ReactNode } from "react";

export function Kicker({ children }: { children: ReactNode }) {
  return <p className="mb-4 font-sans text-[0.8125rem] font-bold uppercase tracking-[0.12em] text-accent">{children}</p>;
}

export function Headline({ children }: { children: ReactNode }) {
  return (
    <h1 className="mb-6 max-w-[16ch] font-serif text-5xl font-bold leading-none tracking-tight text-ink md:text-7xl">
      {children}
    </h1>
  );
}

export function Deck({ children }: { children: ReactNode }) {
  return <p className="mb-8 max-w-[38ch] font-serif text-xl leading-snug text-muted md:text-2xl">{children}</p>;
}

export function Byline({ children }: { children: ReactNode }) {
  return <p className="mb-16 border-b border-rule pb-6 font-sans text-sm text-muted">{children}</p>;
}

export function Body({ children }: { children: ReactNode }) {
  return (
    <div className="mx-auto max-w-[66ch] font-serif text-lg leading-[1.7] text-ink [&>p]:mb-5 [&>p:first-of-type]:first-letter:float-left [&>p:first-of-type]:first-letter:mr-3 [&>p:first-of-type]:first-letter:text-7xl [&>p:first-of-type]:first-letter:font-bold [&>p:first-of-type]:first-letter:leading-[0.85] [&>p:first-of-type]:first-letter:text-accent">
      {children}
    </div>
  );
}

export function PullQuote({ children }: { children: ReactNode }) {
  return (
    <blockquote className="mx-auto my-[72px] max-w-[30ch] border-y border-rule py-6 text-center font-serif text-3xl italic leading-tight text-ink md:text-4xl">
      {children}
    </blockquote>
  );
}

export function Figure({ src, alt, caption }: { src: string; alt: string; caption: string }) {
  return (
    <figure className="my-20 md:-mx-[12%]">
      <img src={src} alt={alt} className="block aspect-[3/2] w-full object-cover" loading="lazy" />
      <figcaption className="mx-auto mt-3 max-w-[66ch] font-sans text-sm text-muted">{caption}</figcaption>
    </figure>
  );
}
```

## Page (EditorialPage.tsx)
```tsx
import { Body, Byline, Deck, Figure, Headline, Kicker, PullQuote } from "./Article";

export default function EditorialPage() {
  return (
    <main className="bg-paper px-6 pb-[120px] pt-24">
      <article className="mx-auto max-w-[1120px]">
        <Kicker>Long read</Kicker>
        <Headline>The quiet return of the printed page</Headline>
        <Deck>Readers are choosing slower formats, and designers are rediscovering what a well-set column can do.</Deck>
        <Byline>By Staff Writer · 12 min read</Byline>

        <Body>
          <p>Every good page begins with a decision about how long a line should be. For most readers, the answer is somewhere near seventy characters.</p>
          <p>The rest follows from that choice: the leading, the paragraph spacing, the rhythm that lets the eye move without effort.</p>
        </Body>

        <PullQuote>Comfort is the first form of respect a page can offer.</PullQuote>

        <Body>
          <p>Images are placed where they change the pace of the story, not where they simply fill space.</p>
        </Body>

        <Figure
          src="https://images.unsplash.com/photo-1495020689067-958852a7765e?w=1600"
          alt="A stack of printed magazines on a wooden desk"
          caption="Printed editions still set the standard for pacing and hierarchy."
        />
      </article>
    </main>
  );
}
```

## Rules for implementation
- Keep the body column at `max-w-[66ch]` for a comfortable measure.
- Use the accent colour only for kickers, drop caps and pull quotes.
- Pass meaningful `alt` text for every figure and caption for every image.
- Keep one `h1` per article.
