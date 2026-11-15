---
name: progress-indicators-instructions
description: Expert design instructions for progress indicators, covering determinate versus indeterminate patterns, multi-step progress, time estimates, perceived-speed principles and accessible progress announcements. Use when designing or reviewing progress bars, spinners, step indicators or upload and processing feedback.
---

# Progress Indicators: Design Instructions

## Core principle
Progress tells the user that work is happening, how far along it is, and roughly how long remains. Choose the pattern by what you know. Honest progress builds trust; fake progress erodes it.

## Choosing the pattern
- Determinate (known total): a linear progress bar with a percentage or fraction. Use for uploads, file processing and multi-item operations.
- Indeterminate (unknown duration): a spinner or a sweeping bar. Use only for short waits, and pair with a text description of what is happening.
- Multi-step process: a step indicator showing completed, current and upcoming steps (for example onboarding or checkout).
- Long background job: a task card that can be minimised, with a clear completion state and notification.

## Rules for determinate progress
1. Progress must reflect real work. Do not advance a bar on a timer when the operation has no measurable steps.
2. Show a numeric value alongside the bar (for example 64 percent or 3 of 5 files).
3. Provide an estimated time remaining only when it is reliable; update it smoothly, not every frame.
4. Never let progress reach 100 percent before the operation has finished.

## Rules for indeterminate progress
- Keep spinners small and in context (near the button or section being loaded).
- Use text labels such as "Processing your report" rather than a spinner alone.
- If the wait exceeds about 10 seconds, show more detail or an option to continue elsewhere.

## Perceived speed
- Show feedback immediately, even if the response takes time.
- Advance the first step visibly to show the process has started.
- Avoid progress bars that stall at 99 percent for long periods.

## Accessibility
- Use a native `<progress>` element or `role="progressbar"` with `aria-valuenow`, `aria-valuemin`, `aria-valuemax` and an accessible name.
- Announce major milestones through a live region, not every percentage change.
- Do not rely on colour alone; include the numeric value and a text status.
- Respect reduced motion by making spinners static or replacing them with a pulsing label.

## Anti-patterns
Fake progress that jumps, spinners with no text for long waits, progress bars that reset without explanation, unlabelled step indicators, and loading states that block the entire screen when only one part is loading.
