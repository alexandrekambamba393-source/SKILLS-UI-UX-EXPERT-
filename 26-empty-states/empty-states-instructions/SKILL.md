---
name: empty-states-instructions
description: Expert design instructions for empty states, covering first-use, no-results, cleared-content and error-driven empty screens, with guidance on explaining value, offering a clear next action, tone and accessibility. Use when designing or reviewing screens shown when there is no data, no search results or no items to display.
---

# Empty States: Design Instructions

## Core principle
An empty state is an opportunity to teach and to act. It explains why the screen is empty, what the user can do next, and what will appear once they act. An empty screen with no explanation looks broken.

## Types of empty state
1. First use: the user has not created anything yet. Explain the value of the feature and offer the first action.
2. No results: a search or filter returned nothing. Explain what was searched, suggest adjustments, and offer to clear filters.
3. Cleared or completed: the user has finished everything (inbox zero, all tasks done). Acknowledge the achievement briefly and point to the next useful action.
4. Error or unavailable: data could not load or the feature is not available. Explain briefly, and offer a retry or an alternative path.
5. Permission-restricted: the user cannot see content. Explain who can grant access and how to request it.

## Structure
- Illustration or icon (optional, small, and not decorative noise).
- Headline: states the situation in plain words ("No projects yet").
- Supporting line: one or two sentences explaining the value or the reason.
- Primary action: one clear button ("Create a project").
- Secondary action (optional): a link to documentation or a template.

## Writing
- Use the user's goal in the headline, not system terminology.
- Avoid blame: "No results for 'budget'" rather than "You did not find anything".
- Keep the tone warm and direct. Never use humour when the user may be stressed, such as in error or permission states.
- Suggest a concrete step, such as "Try a shorter search term" or "Remove one filter".

## Design
- Keep the empty state centred within its content area, with generous spacing.
- Use a neutral background and a dashed or subtle border to show absence without alarm.
- Do not use the empty state to hide a broken layout; fix the underlying loading issue first.

## Accessibility
- Make the headline a real heading level so screen reader users can find the state.
- Ensure the primary action is focusable and labelled clearly.
- Announce results changes (for example after a filter) through a live region.
- Do not rely on illustration alone to convey meaning.

## Anti-patterns
A blank white screen, a generic "Nothing here" with no action, a "No results" message with no way to change the search, playful copy in error states, and empty states that look like loading states.
