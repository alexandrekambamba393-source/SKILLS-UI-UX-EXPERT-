---
name: dark-mode-instructions
description: Expert design instructions for dark mode interfaces, covering contrast-safe surfaces, elevation through lightness, accent desaturation and theme switching. Use when designing, auditing or adding a dark theme to a product.
---

# Dark Mode: Design Instructions

## Core principle
A dark interface is not an inverted light one. Depth comes from surface lightness steps, not from shadows. Color must be tuned for dark backgrounds, where saturated hues vibrate and pure white causes glare.

## Surface and elevation model
1. Base background: a near-black with a slight cool or warm tint (for example #0E1116), never pure #000 for large areas.
2. Elevation: each raised level is about 4 to 6 percent lighter (#161B22, #1E242C, #272E38).
3. Text: primary at about 92 percent white, secondary at 65 percent, disabled at 40 percent. Never pure #FFF for body text.
4. Borders: use a 1px line about 10 percent lighter than the surface.

## Color rules
- Desaturate accents by 10 to 20 percent versus the light theme to reduce vibration.
- Verify every semantic color (success, warning, danger) on the darkest surface you use.
- Keep brand identity by holding hue constant while shifting lightness.
- Charts need palettes checked for colorblind safety in dark mode, not only light mode.

## Contrast and accessibility
- Body text at 4.5:1 minimum; large text and UI components at 3:1 (WCAG 2.2 AA).
- Test focus rings: the accent must reach 3:1 against the surface.
- Images with a light background should be dimmed (filter brightness 0.85 to 0.9) to avoid a glowing rectangle.

## Theme behavior
- Respect the system preference via prefers-color-scheme on first visit.
- Offer a manual toggle that persists the choice and overrides the system setting.
- Switch themes without a flash: set the attribute before first paint.
- Keep layout, spacing and typography identical across themes; only color tokens change.

## Anti-patterns
Pure black on pure white inversions, saturated neon accents on black, gray text on gray cards with little contrast, shadows used for elevation (they disappear on dark), and images that were not adapted.
