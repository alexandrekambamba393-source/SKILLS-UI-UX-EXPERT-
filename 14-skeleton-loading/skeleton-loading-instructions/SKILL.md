---
name: skeleton-loading-instructions
description: Expert design instructions for skeleton loading screens, covering layout-matching placeholders, shimmer timing, perceived-performance principles, transition to real content and accessibility for loading states. Use when designing or reviewing placeholder states shown while content is being fetched.
---

# Skeleton Loading: Design Instructions

## Core principle
A skeleton previews the shape of the content that is coming. It reduces perceived wait time and prevents layout shift. It is a promise of structure, not a decoration.

## Matching the layout
1. The skeleton must mirror the final layout: same grid, same card sizes, same text line counts where possible.
2. Use rounded rectangles for text lines (height about 0.75em of the final text), circles for avatars, and blocks for images at their final aspect ratio.
3. Vary line widths slightly (for example 100%, 92%, 70%) so the placeholder feels like text, not a barcode.
4. Show the skeleton only for the regions that are actually loading. Keep the header and navigation real.

## Timing and perception
- Show the skeleton after a short delay (about 150ms to 300ms). Fast responses should not flash placeholders.
- Keep the skeleton visible for at least around 300ms once shown, so it does not flicker.
- Use a shimmer or gentle pulse at about 1.2s to 1.8s per cycle. Faster motion feels anxious; slower motion feels stalled.
- Transition to real content with a short fade (150ms to 200ms).

## Colour and contrast
- Use a neutral grey pair: a base tone and a slightly lighter highlight. Keep it low-contrast so it does not compete with real content.
- Works in light and dark modes; define both tones as tokens.

## Accessibility
- Mark the loading region with `aria-busy="true"` and provide a text status for screen readers ("Loading orders").
- Hide decorative skeleton blocks from assistive technology (`aria-hidden="true"`).
- Respect `prefers-reduced-motion` by replacing the shimmer with a static placeholder.
- When loading fails, replace the skeleton with a clear error and a retry action; never leave a skeleton running indefinitely.

## Anti-patterns
Generic grey boxes that do not match the layout, skeletons for loads that finish in under 150ms, shimmer that loops for minutes, skeletons on top of a blank page with no header, and skeletons without an error fallback.
