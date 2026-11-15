---
name: parallax-scrolling-instructions
description: Expert design instructions for parallax scrolling, covering layer depth models, speed ratios, performance-safe implementation, storytelling pacing and accessibility safeguards including vestibular motion sensitivity. Use when designing or reviewing scroll-driven layered motion in landing pages, storytelling pages or hero sections.
---

# Parallax Scrolling: Design Instructions

## Core principle
Parallax simulates depth by moving background layers more slowly than foreground layers. It works best as an atmospheric effect that supports a narrative. It must never make the content harder to read or the page harder to use.

## Depth model
1. Define three to four layers: far background, mid background, content plane, foreground accents.
2. Assign speed factors that increase with proximity: far 0.2x, mid 0.4x, content 1x (no movement), foreground 1.2x.
3. Keep the content plane static relative to the page. Only decorative layers should move.
4. Limit the number of moving layers to three or four per section.

## Motion rules
- Movement is smooth and tied to scroll position; avoid jitter and delayed snapping.
- Use transform (translate3d) rather than top or margin for motion.
- Keep total displacement modest, typically 40px to 120px per layer across a section.
- Avoid parallax on text-heavy sections; use it on hero and transition sections.

## Performance
- Use `will-change: transform` sparingly and only on animated layers.
- Prefer CSS scroll-driven animations (animation-timeline: scroll) where supported; fall back to requestAnimationFrame-throttled scroll listeners.
- Avoid heavy images moving independently on mobile; reduce or disable effects below 768px.

## Accessibility
- Parallax can trigger vestibular disorders. Disable it entirely when `prefers-reduced-motion` is set.
- Never move text that users must read at the same time as its background.
- Ensure keyboard navigation and focus remain stable when layers move.
- Provide a static, complete version of every section without motion.

## Storytelling and pacing
- Align parallax with narrative beats: one scene per viewport height.
- Pair motion with a clear reading path; the eye should always know where to go next.
- Let the content settle before the next layer moves; do not stack many effects at once.

## Use and avoid
Use for brand landing pages, product storytelling, editorial features and campaigns. Avoid for data dashboards, forms, checkout flows and any page where scanning speed is critical.

## Anti-patterns
Parallax on every section, text that drifts over busy backgrounds, heavy full-resolution images moving on mobile, effects without reduced-motion fallbacks, and scroll hijacking that removes native scrolling control.
