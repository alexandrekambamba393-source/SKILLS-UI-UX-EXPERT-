---
name: neumorphism-instructions
description: Expert design instructions for neumorphism (soft UI), covering dual-shadow relief, tonal consistency, contrast safeguards and when to avoid the style. Use when designing, reviewing or critiquing soft-relief interface components such as buttons, toggles and inset controls.
---

# Neumorphism: Design Instructions

## Core principle
Components appear extruded from or pressed into a single background colour. Relief is created by two shadows of the same surface colour: one light, one dark, offset in opposite directions. The surface and the component must share one hue.

## Construction rules
1. Choose a mid-light base colour (for example #e0e5ec). Do not use white or pure saturated colours.
2. Raised elements: `box-shadow: 8px 8px 16px dark, -8px -8px 16px light`.
3. Pressed or inset elements: use `inset` with the same two shadows, reduced in intensity.
4. Shadow intensity is a function of size: small controls use 4 to 6px offsets, large panels use 10 to 16px.
5. Radius: 12px to 24px. Sharp corners break the soft-relief illusion.
6. Keep one light source direction across the whole interface (top-left).

## State language
- Default: raised.
- Hover: slightly lifted (larger shadows).
- Active or pressed: inset.
- Selected toggle: inset with an accent-coloured indicator, not a colour change alone.
- Disabled: flat, with reduced contrast.

## Accessibility safeguards
- Shadows alone do not define boundaries; WCAG 2.2 non-text contrast requires 3:1 for control boundaries and states. Add a border or an accent-coloured indicator when the shadow contrast is insufficient.
- Text on the base colour must reach 4.5:1. Mid-grey text on grey surfaces fails most of the time.
- Focus rings must be visible and independent of shadow (use a 2px solid outline).

## Use and avoid
Use for small sets of controls in dashboards, media players, settings panels and calculators. Avoid for dense data, long forms, navigation with many items, or any product where users must distinguish many states quickly.

## Anti-patterns
Multiple background colours inside one component, excessively dark shadows, relief applied to every element, and neumorphic controls without any focus or state indicator.
