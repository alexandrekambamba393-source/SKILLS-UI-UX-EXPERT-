---
name: frictionless-ux-instructions
description: Expert design instructions for frictionless user experience, covering reduction of steps and cognitive load, smart defaults, autofill and progressive commitment, one-click paths, error prevention and measuring friction. Use when designing or reviewing flows such as checkout, sign-up, account actions or any multi-step task that should be as quick as possible without sacrificing correctness.
---

# Frictionless UX: Design Instructions

## Core principle
Friction is any effort that does not serve the user's goal. Remove the unnecessary and keep the necessary. Necessary friction (confirming a payment, deleting data) protects the user; unnecessary friction (repeated entry, hidden steps, unclear choices) only costs them time and trust.

## Audit the flow
1. Map every step from the user's starting point to completion, counting clicks, fields, decisions and waits.
2. Classify each step: needed for the goal, needed for safety, or unnecessary.
3. Remove unnecessary steps first. Then reduce the effort of the remaining steps.
4. Measure drop-off at each step to find where users actually give up.

## Techniques to reduce friction
- Smart defaults: pre-select the most common option, and let users change it.
- Autofill and remembered data: use correct autocomplete attributes and offer saved values.
- Progressive commitment: ask for the least information first, then collect more only when needed.
- Single-page flows: keep related steps on one screen when the total is small.
- Inline editing: change a value where it is shown instead of opening a separate form.
- Guest or deferred sign-up: let users complete the core task before creating an account, where the product allows.
- One-tap repeat actions: offer "same as last time" for recurring tasks.
- Clear next step: every screen states the single most likely next action.

## Keep necessary friction
- Confirm irreversible or costly actions (deletion, payment, sending to many people), with a specific confirmation.
- Use undo for reversible actions instead of confirmation dialogs whenever possible; undo is less friction and equally safe.
- Show a summary before final submission in high-stakes flows.

## Reduce cognitive load
- Limit choices on each screen; group options by decision.
- Use plain language and familiar patterns.
- Avoid asking users to remember information across screens.

## Accessibility and inclusion
- Smart defaults must be visible and reversible.
- Autofill must work with password managers and assistive technology.
- Do not remove friction at the expense of error handling; invalid input still needs clear guidance.
- Keep touch and keyboard paths equally short.

## Measurement
- Track completion rate, time to complete, error rate per field and abandonment step.
- Test changes with real users; a faster flow that causes more errors is not an improvement.

## Anti-patterns
Asking for information the product already has, forcing account creation before any value, hiding the next step, confirmation dialogs for every action, and optimisations that remove safeguards for high-stakes actions.
