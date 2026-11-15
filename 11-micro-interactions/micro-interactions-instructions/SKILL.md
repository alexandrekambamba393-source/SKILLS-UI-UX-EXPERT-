---
name: micro-interactions-instructions
description: Expert design instructions for micro-interactions, covering the trigger-rules-feedback-loops-mode model, timing and easing standards, feedback for clicks, hovers and state changes, and reduced-motion accessibility. Use when designing or reviewing small animated responses to user actions such as toggles, likes, form submissions and button presses.
---

# Micro-interactions: Design Instructions

## Core principle
A micro-interaction answers a single question for the user: "Did my action register, and what happened?" Each one is small, purposeful and fast. Delight is a side effect of clarity, never the goal on its own.

## The four parts (Dan Saffer model)
1. Trigger: what starts it (user action or system event).
2. Rules: what the interaction does.
3. Feedback: what the user sees, hears or feels.
4. Loops and modes: whether it repeats, and whether it behaves differently in different contexts.

Define all four before building anything.

## Timing standards
- Instant feedback (press state, colour change): under 100ms.
- Standard transitions (toggle, expand, fade): 150ms to 250ms.
- Larger spatial changes (sheets, drawers, page sections): 250ms to 400ms.
- Anything above 500ms must communicate progress.
- Use ease-out for entering elements and ease-in for exiting ones. Avoid linear easing for UI.

## Feedback patterns
- Buttons: a visible press state (scale 0.98 or a darker fill) within 100ms.
- Toggles and checkboxes: the state change animates in 150ms to 200ms and the label updates.
- Likes or favourites: a small scale-up and a colour change, with the count updating immediately.
- Form submission: disable the button, show a spinner in place of the label, then a clear success or error state.
- Hover affordances on desktop only; never make hover the only way to discover an action.

## Accessibility
- Honour `prefers-reduced-motion`: replace movement with fades or instant state changes.
- Do not rely on animation alone to convey meaning; the final state must be readable without motion.
- Announce asynchronous outcomes to screen readers using `aria-live` regions.
- Keep focus indicators unaffected by animation.

## Restraint
- Limit each screen to a small number of simultaneous animations.
- Avoid animating layout properties such as width and height on frequently updated elements; prefer transform and opacity.
- Micro-interactions should never block input. Users must be able to act during an animation.

## Anti-patterns
Animations longer than 500ms for simple feedback, bouncy motion on serious tasks such as payments or deletions, decorative motion with no state change, animation that replays on every page visit, and interactions that break keyboard use.
