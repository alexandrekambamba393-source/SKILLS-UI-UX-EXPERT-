---
name: user-onboarding-instructions
description: Expert design instructions for user onboarding, covering first-run activation, progressive disclosure, checklists, contextual tooltips, value-first sign-up, skippable tours and measuring activation. Use when designing or reviewing the first experience a new user has with a product, including sign-up flows, welcome tours and setup checklists.
---

# User Onboarding: Design Instructions

## Core principle
Onboarding exists to get the user to their first moment of value as quickly as possible. Every step must earn its place by moving the user toward that moment. Teach by doing, not by describing.

## Define activation first
1. Identify the single action or outcome that correlates with long-term retention (the activation event), for example "created first project" or "invited a teammate".
2. Work backwards: remove every step that does not lead to that event.
3. Measure time to activation and drop-off at each step.

## Flow design
- Delay sign-up as long as possible: let users try the core value first where the product allows.
- Ask only for information needed right now. Each extra field reduces completion.
- Use progressive disclosure: introduce features when they are relevant, not all at once.
- Offer sample data or a template so the first screen is never empty.
- Use an empty state that explains what will appear and how to create it.

## Guidance patterns
- Setup checklist: three to five items, visible in the app, showing progress. Items complete through real actions.
- Contextual tooltips: appear next to the feature the first time it is relevant; dismissible and never repeated.
- Product tours: keep them short (three steps or fewer), skippable at any point, and replayable from help.
- Welcome message: short, specific, and focused on the next action.

## Respect and control
- Always provide a skip option. Forced tours increase abandonment.
- Remember what the user has seen so guidance does not repeat.
- Make onboarding dismissible without losing progress.
- Do not interrupt active work with modal tours.

## Copy and tone
- Use the user's goal in headings ("Set up your first project") rather than the product's feature names.
- Keep instructions to one action per step.
- Avoid jargon; explain necessary terms in place.

## Accessibility
- Tours must be keyboard-navigable and announce each step to screen readers.
- Do not rely on animated highlights alone to indicate what to do.
- Keep checklist completion states conveyed in text, not only icons.

## Measurement
- Track activation rate, time to first value, step completion and skip rate.
- Test changes with controlled rollouts; onboarding changes can reduce retention if they optimise only for sign-up.

## Anti-patterns
Long mandatory tours, requesting many fields at sign-up, empty dashboards with no next step, tooltips that cover the interface, and onboarding that cannot be revisited.
