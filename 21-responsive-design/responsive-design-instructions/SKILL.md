---
name: responsive-design-instructions
description: Expert design instructions for responsive web design, covering mobile-first breakpoints, fluid typography and spacing, content priority across screen sizes, touch and pointer adaptation and testing discipline. Use when designing or reviewing layouts that must adapt across phones, tablets, laptops and large displays.
---

# Responsive Design: Design Instructions

## Core principle
One product, many contexts. Design the content hierarchy first, then let the layout adapt. A responsive interface reorders and reflows content rather than shrinking it, and it respects the input method of each device.

## Mobile-first method
1. Start with the smallest viewport (360px wide) and add complexity as space grows.
2. Define three to four breakpoints based on content, not on device names. Typical starting points: 480px, 768px, 1024px, 1280px.
3. Ensure a single column works at 360px before adding multi-column layouts.
4. Test with real content lengths, including long names, translations and error messages.

## Layout
- Use CSS Grid and Flexbox with `minmax()`, `auto-fit` and `clamp()` rather than fixed pixel widths.
- Keep line length between 45 and 75 characters on all sizes; avoid full-width text on large screens.
- Reorder content by priority on small screens. The primary action should be visible without excessive scrolling.
- Navigation becomes a menu or bottom bar on mobile, with a clear open and close affordance.

## Typography and spacing
- Use fluid type with `clamp()`, for example `clamp(1rem, 0.9rem + 0.4vw, 1.125rem)` for body text.
- Keep body text at 16px minimum on mobile to avoid automatic zoom on iOS input fields.
- Scale spacing with the viewport; generous on desktop, compact but comfortable on mobile.

## Touch and pointer
- Touch targets at least 44px by 44px; space adjacent targets at least 8px apart.
- Avoid hover-only interactions; provide tap-friendly alternatives.
- Use `@media (pointer: coarse)` to enlarge targets on touch devices and `@media (hover: hover)` for hover enhancements.

## Media and performance
- Serve images at multiple resolutions with `srcset` and `sizes`; use modern formats where supported.
- Lazy-load below-the-fold media.
- Avoid large layout shifts: set width and height or aspect-ratio on media.

## Accessibility
- Support zoom to 200 percent and text resizing without loss of content or function (WCAG 2.2 Reflow at 320px width without horizontal scrolling).
- Keep focus order consistent across breakpoints.
- Do not hide critical controls on mobile; relocate them.

## Testing
- Test at 360px, 768px, 1024px and 1440px minimum.
- Test in portrait and landscape on tablets and phones.
- Check with browser zoom at 200 percent and with the system font size increased.

## Anti-patterns
Desktop layouts shrunk onto phones, fixed-width containers, hidden essential content on mobile, text too small to read without zoom, hover-only menus, and images that cause layout shift.
