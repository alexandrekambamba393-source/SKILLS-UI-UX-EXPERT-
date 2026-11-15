---
name: glassmorphism-instructions
description: Expert design instructions for glassmorphism, covering translucent layers, backdrop blur, light edges, layered depth and legibility safeguards. Use when designing or reviewing frosted-glass cards, panels or overlays.
---

# Glassmorphism: Design Instructions

## Core principle
Glass is a material that shows what is behind it. It only works when there is a colorful or textured background to refract. Use it to create hierarchy and depth, never as decoration on a flat white page.

## Material recipe
1. Background: a layered gradient or soft shapes behind the glass (the glass has nothing to show otherwise).
2. Surface fill: white or tinted at 10 to 25 percent opacity.
3. Blur: backdrop-filter blur between 12px and 24px. Higher values cost performance.
4. Edge: a 1px border at 20 to 35 percent white, plus a subtle inner highlight on the top edge.
5. Shadow: soft and large (for example 0 8px 32px rgba(0,0,0,0.12)) to lift the panel.
6. Corners: 16px to 24px radius.

## Legibility rules
- Text on glass must pass 4.5:1 against the blended color. Test the worst-case background behind the panel.
- Never place small text (under 14px) directly on busy imagery without a tint layer.
- Increase surface opacity when content is dense; lower it only for decorative panels.
- Reserve glass for layers above content (cards, navigation, modals), not for every element.

## Performance and fallback
- Limit simultaneously blurred layers to two or three on mobile.
- Provide a solid fallback for browsers without backdrop-filter support.
- Respect prefers-reduced-transparency where it is available by replacing glass with solid surfaces.

## Accessibility
- Glass must not be the only way to convey a state; pair with icons and labels.
- Focus rings must stay opaque and high contrast.

## Anti-patterns
Glass on a plain white background, text at low opacity over animated backgrounds, stacking more than three blurred layers, and glass applied to every component so that nothing stands out.
