---
name: tactile-feedback-instructions
description: Expert design instructions for tactile feedback in interfaces, covering press depth, haptic patterns for touch devices, timing of press and release, visual mimicry of physical buttons and accessibility considerations. Use when designing or reviewing how buttons and controls respond to touch and press, especially in mobile apps and touch-first web interfaces.
---

# Tactile Feedback: Design Instructions

## Core principle
Every press must be acknowledged within the time a finger takes to make contact. The interface should feel like a physical control: it responds at the moment of pressure, confirms the action on release, and returns to rest. Tactile feedback reduces uncertainty, especially on touch screens where there is no hover.

## Visual response to press
1. Press down: scale to 0.96 to 0.98, or darken the fill by 6 to 10 percent. Respond within 60ms to 100ms.
2. Hold: maintain the pressed state while the finger is down. Do not animate on hold.
3. Release: return to rest with a spring or ease-out curve of 150ms to 200ms.
4. Cancel: if the finger slides off the control, return to rest without triggering the action.
5. Keep the pressed area stable: the control must not shift layout while pressed.

## Haptic feedback (touch devices)
- Light impact for toggles and selection changes.
- Medium impact for confirming an action (send, save).
- Notification pattern (success, warning, error) for outcomes that matter.
- Use haptics sparingly. Pair them with a visual change; do not rely on haptics alone.
- Respect the device setting: haptics are optional and can be disabled.

## Web implementation
- Use `:active` for press feedback. It is the closest web equivalent to touch-down.
- Use the Vibration API (`navigator.vibrate`) only where supported and only with short patterns, such as 10ms for a light tap. Note that it is unavailable on iOS Safari.
- Never use vibration as the only feedback signal.

## Touch targets and comfort
- Minimum touch target 44px by 44px (WCAG 2.2 target size minimum 24px; 44px is recommended for touch).
- Provide at least 8px spacing between adjacent targets.
- Keep primary actions within thumb reach on mobile.

## Accessibility
- Respect `prefers-reduced-motion` by replacing scale motion with a colour change.
- Keep the focus ring visible for keyboard users; pressed state does not replace focus.
- Ensure the pressed state still meets 3:1 non-text contrast against the background.

## Anti-patterns
Long delays between touch and response, animation that moves layout under the finger, haptics on every tap, tactile effects on non-interactive surfaces, and press states with no release state.
