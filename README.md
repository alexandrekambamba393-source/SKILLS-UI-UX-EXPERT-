# SKILLS UI-UX EXPERT

A collection of 30 expert-level UI/UX skills for AI assistants and developers. Each skill set covers one design style, pattern or principle, and gives you two things: **expert design instructions** and **production-ready code**.

The collection is written entirely in English and is designed to be dropped into any Agent Skills–compatible tool (for example Claude Skills), or read directly as a reference handbook.

---

## Table of contents

- [What is in this repository](#what-is-in-this-repository)
- [Repository structure](#repository-structure)
- [The 30 styles and patterns](#the-30-styles-and-patterns)
- [How each style is organised](#how-each-style-is-organised)
- [How to use the skills](#how-to-use-the-skills)
- [Code variants](#code-variants)
- [Design principles behind the collection](#design-principles-behind-the-collection)
- [Standards referenced](#standards-referenced)
- [Quality and limitations](#quality-and-limitations)
- [Contributing](#contributing)
- [License](#license)

---

## What is in this repository

Every style lives in its own folder and contains three skills:

| Skill | Purpose |
| --- | --- |
| `<style>-instructions` | Expert guidance: core principle, rules, timing, accessibility, when to use the style and when to avoid it, and anti-patterns. |
| `<style>-code` | A complete, self-contained **HTML and CSS** implementation (with vanilla JavaScript where interaction is needed). |
| `<style>-code-react-tailwind` | The same pattern as **React components** with **Tailwind CSS** configuration. |

That makes **90 skills** across **30 folders**.

Each skill is a `SKILL.md` file with YAML frontmatter (`name` and `description`) followed by Markdown content. The `description` field tells an assistant when to use the skill.

---

## Repository structure

```
SKILLS UI-UX EXPERT/
├── README.md
├── 01-minimalism/
│   ├── minimalism-instructions/
│   │   └── SKILL.md
│   ├── minimalism-code/
│   │   └── SKILL.md
│   └── minimalism-code-react-tailwind/
│       └── SKILL.md
├── 02-dark-mode/
│   └── ...
├── ...
└── 30-contextual-feedback/
    └── ...
```

Folders are numbered `01` to `30` so they sort in a predictable order.

---

## The 30 styles and patterns

### Visual styles
| # | Folder | Topic |
| --- | --- | --- |
| 01 | `01-minimalism` | Clean, reduced interfaces with generous whitespace |
| 02 | `02-dark-mode` | Dark themes, contrast-safe surfaces, theme switching |
| 03 | `03-glassmorphism` | Translucent, blurred glass panels |
| 04 | `04-neumorphism` | Soft, extruded and inset controls |
| 05 | `05-brutalism` | Bold, raw, neo-brutalist layouts |
| 06 | `06-swiss-design` | Grid-driven, typographic International Style |
| 07 | `07-bento-grid` | Modular cards of varying sizes |
| 08 | `08-gradient-design` | Gradients for depth and identity |
| 09 | `09-editorial-design` | Magazine-style long-form layouts |
| 10 | `10-neo-corporate` | Professional, trustworthy SaaS interfaces |

### Interaction and animation
| # | Folder | Topic |
| --- | --- | --- |
| 11 | `11-micro-interactions` | Small, purposeful feedback animations |
| 12 | `12-parallax-scrolling` | Layered depth on scroll |
| 13 | `13-scroll-reveal` | Content that appears as the user scrolls |
| 14 | `14-skeleton-loading` | Placeholders that preview incoming content |
| 15 | `15-page-transitions` | Continuity between views and routes |
| 16 | `16-hover-effects` | Hover states with keyboard and touch parity |
| 17 | `17-data-visualization` | Accessible charts and KPI tiles |
| 18 | `18-custom-cursor` | Enhanced pointers for fine-pointer devices |
| 19 | `19-tactile-feedback` | Press and release feedback for touch |
| 20 | `20-progress-indicators` | Determinate and indeterminate progress |

### UX principles
| # | Folder | Topic |
| --- | --- | --- |
| 21 | `21-responsive-design` | Mobile-first, fluid, reflow-safe layouts |
| 22 | `22-accessibility-wcag` | WCAG 2.2 AA patterns |
| 23 | `23-intuitive-navigation` | Information architecture and wayfinding |
| 24 | `24-user-onboarding` | First-run experience and activation |
| 25 | `25-smart-form-validation` | Timely, accessible form validation |
| 26 | `26-empty-states` | Empty, no-results and error screens |
| 27 | `27-design-system` | Tokens, theming and component governance |
| 28 | `28-frictionless-ux` | Reducing effort without removing safeguards |
| 29 | `29-command-palette` | Keyboard-driven command overlays |
| 30 | `30-contextual-feedback` | Toasts, banners and inline messages |

---

## How each style is organised

Every instructions skill follows the same outline:

1. **Core principle**: the one idea the style or pattern is built on.
2. **Rules and process**: concrete, checkable guidance.
3. **Timing, colour or typography standards** where relevant.
4. **Accessibility**: how the pattern must behave for keyboard, screen reader, low vision and reduced-motion users.
5. **Use and avoid**: when the pattern fits and when it does not.
6. **Anti-patterns**: common mistakes to catch in review.

Every code skill follows the same outline:

1. A complete, copy-ready example.
2. **Rules for implementation**: the constraints the example respects, so you can extend it safely.

---

## How to use the skills

### With an Agent Skills–compatible assistant
Copy the folder of the skill you need (or the whole repository) into your skills directory. The assistant reads the `description` in each frontmatter to decide when to load it. For example, a request to "build an accessible modal" should load `22-accessibility-wcag`.

### As a reference
Open the `*-instructions/SKILL.md` file before designing a screen. Use it as a review checklist. Then open the matching code skill to implement it.

### As a starting point for a project
- **Plain web project**: start from a `*-code/SKILL.md` example. Each file is a standalone HTML document you can open in a browser.
- **React project**: start from a `*-code-react-tailwind/SKILL.md` file. Merge the Tailwind configuration into your `tailwind.config.js`, then copy the components.

### Combining styles
Many screens use several patterns together. For example, a dashboard might combine `10-neo-corporate`, `17-data-visualization`, `20-progress-indicators`, `26-empty-states` and `30-contextual-feedback`. Use the instructions skills to decide the combination, and the code skills to build each part.

---

## Code variants

| Variant | Best for | Requirements |
| --- | --- | --- |
| HTML and CSS (with vanilla JavaScript where needed) | Static sites, prototypes, learning, any framework | A modern browser |
| React and Tailwind CSS | React, Next.js and other component-based apps | React 18+, Tailwind CSS v3 |

Notes:
- The HTML examples use CSS custom properties for theming. Change the `:root` variables to match your brand.
- The React examples assume the Tailwind configuration shown in each skill. Some examples use arbitrary values (for example `outline-[3px]`) that work in Tailwind v3 without extra plugins.
- Some HTML examples load a sample image from Unsplash. Replace these URLs with your own assets before production use.

---

## Design principles behind the collection

These principles apply across all 30 skills:

1. **Clarity over decoration.** Every visual choice must help the user understand or act.
2. **Accessibility is the baseline.** Keyboard, screen reader, contrast and reduced-motion support are built into every example, not added afterwards.
3. **Feedback is always visible.** Every action has an observable result, in text as well as in motion or colour.
4. **Progressive enhancement.** Core content works without JavaScript or advanced CSS, and enhancements are layered on top.
5. **Respect for the user's settings.** Reduced motion, colour scheme, text size and input method are honoured.
6. **Reuse through tokens.** Colour, spacing, radius and motion are expressed as named values so systems stay consistent.

---

## Standards referenced

- **WCAG 2.2** (Web Content Accessibility Guidelines), Level AA: contrast, focus appearance, target size, reflow, reduced motion and more.
- **WAI-ARIA Authoring Practices**: combobox, listbox, dialog and live-region patterns.
- **CSS View Transitions** and **scroll-driven animations**: used with fallbacks where supported.
- **Micro-interaction model** (trigger, rules, feedback, loops and modes), as popularised by Dan Saffer.
- **Design tokens** in three tiers (primitive, semantic, component).
- **Modular type scales** and **8px-based spacing** systems.

---

## Quality and limitations

- The examples were written to be correct and accessible, and the structure of every file was checked. They have **not all been run in a browser or a React build** as part of this release. Test each example in your own environment before shipping it.
- Visual references in the source material were treated as descriptions. The skills describe the styles and principles; they do not reproduce specific screenshots or third-party designs.
- Design guidance is a starting point, not a substitute for user research, testing with real users, or legal review of accessibility compliance.
- Tailwind class names follow Tailwind v3 conventions. Check them against your version if you use Tailwind v4.

---


---

## License

Copyright (c) 2026 alexandrekambamba393-source

Copyright (c) 2026 Alexandre Kambamba. All rights reserved.

Permission is hereby granted to any person obtaining a copy of this software and associated documentation files (the "Software"), to download and use the Software for personal or internal use, subject to the following conditions:

The Software may NOT be modified, altered, translated, or adapted in any way.
The Software may NOT be used to create derivative works.
Redistribution or sublicensing of modified versions of the Software is strictly prohibited.
THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED.
