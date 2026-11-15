---
name: scroll-reveal-instructions
description: Expert design instructions for scroll reveal animations, covering reveal triggers, stagger rhythm, content-first accessibility, performance-safe IntersectionObserver usage and when reveals harm usability. Use when designing or reviewing elements that fade or slide into view as the user scrolls.
---

# Scroll Reveal: Design Instructions

## Core principle
Content appears as the reader reaches it, guiding attention down the page. Reveals are pacing devices, not entertainment. Content must always be available: if the animation fails, the information must still be there.

## When to use
- Long landing pages and storytelling sections where pacing helps comprehension.
- Feature lists, testimonials and case-study cards arriving in sequence.
- Section headings introducing new content.

## When to avoid
- Dashboards, data tables, forms and checkout flows, where users scan and need instant access.
- Content below the fold that users are likely to search for with find-in-page.
- Anything that delays a primary call to action.

## Motion rules
1. Use small movements: 16px to 32px translation with an opacity fade.
2. Duration: 400ms to 600ms, ease-out.
3. Stagger sibling elements by 60ms to 120ms, capped at about 400ms total so the group does not feel slow.
4. Reveal once. Do not re-hide elements when the user scrolls back up, unless the content is a repeated demonstration.
5. Trigger when the element is about 10 to 20 percent into the viewport, not at the moment it enters.

## Accessibility and robustness
- Respect `prefers-reduced-motion` by showing content immediately or with only a fade.
- Never hide content from assistive technology. Hidden text must not be removed from the accessibility tree.
- Provide a no-JavaScript fallback: content visible by default, with the hidden state applied only when JavaScript is ready.
- Do not animate in content that users must read while it moves.

## Performance
- Use IntersectionObserver, not scroll listeners.
- Animate only transform and opacity.
- Unobserve elements after they reveal to reduce work.

## Anti-patterns
Revealing text in a way that blocks reading, reveals that replay on every scroll, long delays before content appears, hidden links and buttons that cannot be focused, and reveals applied to every single element on the page.
