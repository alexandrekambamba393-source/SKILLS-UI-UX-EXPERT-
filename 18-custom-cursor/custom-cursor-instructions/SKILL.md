---
name: custom-cursor-instructions
description: Expert design instructions for custom cursors on websites, covering when a custom pointer adds value, state-aware cursor feedback, performance-safe following, accessibility and the rule that the native cursor must remain available. Use when designing or reviewing a replacement or enhancement of the system mouse pointer.
---

# Custom Cursor: Design Instructions

## Core principle
A custom cursor is a feedback layer, not a replacement for usability. It should communicate state (hover, drag, loading, text selection) and reinforce the brand, while the native pointer's behaviour remains predictable. If a custom cursor makes the page harder to use, remove it.

## When it adds value
- Portfolios, creative agencies and campaign sites where the pointer is part of the experience.
- Interactive canvases, maps and games where the pointer must indicate a tool or mode.
- Image galleries with drag or zoom affordances.

## When to avoid
- Forms, checkouts, dashboards and any task-driven interface.
- Text-heavy pages where I-beam selection cursors must remain visible.
- Touch devices, where there is no persistent pointer.

## Behavioural rules
1. Keep the native cursor visible for text input and text selection (the I-beam must not be replaced).
2. Change the custom cursor state for interactive targets: a ring that expands on links and buttons, a label for drag or view actions.
3. Hide the custom cursor when it leaves the window or when the pointer is idle for a long time.
4. Do not hide the native cursor entirely unless the custom replacement is at least as clear. Users with low vision rely on system cursor size and colour.

## Motion and performance
- Follow the pointer with a small delay (lerp of 0.15 to 0.25) for a smooth feel, but keep the true position for the dot.
- Use transform and requestAnimationFrame; never trigger layout on pointer move.
- Disable the follow effect for `prefers-reduced-motion`, and disable it entirely on touch devices (`pointer: fine` media query).

## Accessibility
- Custom cursors do not change focus; keyboard navigation and visible focus rings must work independently.
- Do not rely on the custom cursor to convey critical information. Pair it with text labels or inline affordances.
- Avoid cursors that are too small or low contrast against the background.
- Verify that the cursor layer does not intercept clicks: set `pointer-events: none`.

## Anti-patterns
Replacing the cursor on all text, cursors that lag so far behind they feel broken, cursors that hide the native pointer without a visible replacement, custom cursors on touch devices, and cursor layers that block clicks.
