---
name: hover-effects-instructions
description: Expert design instructions for hover effects on cards, buttons and links, covering affordance clarity, state hierarchy, touch-device fallbacks, focus parity and performance-safe transitions. Use when designing or reviewing hover, focus and pointer-driven feedback on interactive elements.
---

# Hover Effects: Design Instructions

## Core principle
Hover tells the user "this is interactive" and "this is the one I am about to act on". It refines discovery; it must never be the only way to find or use something. Every hover state needs an equivalent keyboard focus state.

## Hierarchy of feedback
1. Cursor change (pointer) is always present on clickable elements.
2. Colour or underline change on links: subtle, immediate (100ms to 150ms).
3. Elevation or border change on cards: slight (2px to 4px lift, soft shadow increase).
4. Background tint on buttons and list rows: low-contrast, consistent across the product.
5. Reserve stronger effects (scale, reveal of secondary actions) for the most important controls.

## Timing and easing
- Enter hover: 150ms to 200ms, ease-out.
- Exit hover: 200ms to 250ms, ease-in-out, so the element settles.
- Never delay the hover response; the user must feel the element react.

## Parity and inputs
- Every `:hover` style must have a matching `:focus-visible` style.
- Touch devices have no true hover: use `@media (hover: hover)` for hover-only effects, and give touch users tap feedback through `:active`.
- Do not hide actions behind hover alone (for example, a delete button that appears only on hover). Show it or make it reachable by keyboard.

## Accessibility
- Hover effects must not reduce contrast below 4.5:1 for text.
- Do not use motion that exceeds gentle lift or scale on frequently visible content, especially for users who prefer reduced motion.
- Make sure revealed secondary actions are focusable and announced.

## Performance
- Animate transform, opacity and box-shadow sparingly; prefer transform for lift.
- Avoid hover effects on large lists that trigger layout recalculation.

## Anti-patterns
Hover-only navigation, effects that change layout (pushing neighbours), delays before a hover response, every element reacting equally, and hover states with no focus equivalent.
