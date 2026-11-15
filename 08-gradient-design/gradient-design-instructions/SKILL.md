---
name: gradient-design-instructions
description: Expert design instructions for gradient-driven interfaces, covering colour interpolation, mesh and radial gradients, restrained use for hierarchy and identity, and text legibility over gradients. Use when designing or reviewing backgrounds, hero sections, buttons and accents that rely on gradients.
---

# Gradient Design: Design Instructions

## Core principle
A gradient adds depth and identity when it guides attention. It fails when it becomes the only thing the eye sees. Use gradients for emphasis, atmosphere and brand expression, not for every surface.

## Gradient types and when to use them
- Linear: buttons, progress indicators, dividers. Predictable and easy to read.
- Radial: spotlight effects behind a headline or product. Centre the focal point.
- Mesh (multi-radial blends): hero backgrounds and brand moments. Keep to three or four stops.
- Conic: data visualisation accents and decorative rings. Avoid in dense text layouts.

## Colour rules
1. Choose two or three stops from the same hue family, or adjacent hues on the colour wheel (analogous). Complementary jumps look harsh unless deliberately tense.
2. Interpolate in a perceptual colour space. In CSS, prefer `in oklch` or `in oklab` to avoid muddy midpoints.
3. Keep lightness steps even; large lightness jumps create visible banding and dirty mid-tones.
4. Maintain a neutral or dark anchor so the gradient does not overpower content.

## Legibility
- Test text at the lightest and darkest points of the gradient. Body text must reach 4.5:1 at every point it covers.
- Place text on a solid or low-contrast area of the gradient, or add a scrim.
- Never place small text over high-saturation, high-contrast gradient midpoints.

## Motion
- Animate gradient position or hue slowly (8 to 20 seconds per cycle) when it is decorative.
- Stop or reduce animation for `prefers-reduced-motion` and for users who opt out.
- Animate `background-position` or transform on a pseudo-element; avoid animating heavy full-page gradients on scroll.

## Performance and print
- Gradients are cheap, but animated large areas cost GPU time. Keep them on fixed layers when possible.
- Provide a solid fallback colour for older browsers and for print.

## Anti-patterns
Gradients on every element, rainbow gradients with no brand logic, text over saturated midpoints, muddy grey midpoints from RGB interpolation, and animated backgrounds behind dense content.
