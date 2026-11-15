---
name: smart-form-validation-instructions
description: Expert design instructions for smart form validation, covering when to validate (on blur, on submit, on input), error message writing, inline versus summary errors, accessible error association and recovery patterns. Use when designing or reviewing form validation behaviour, error messages and input guidance in web or mobile forms.
---

# Smart Form Validation: Design Instructions

## Core principle
Validation should prevent errors, catch them early, and explain how to fix them in plain language. Premature validation frustrates users; late validation wastes effort. Timing and wording are the design decisions.

## When to validate
1. Before typing: format hints and input masks (for example "MM/YY" for card expiry) prevent most errors.
2. While typing: only for constraints the user can satisfy incrementally, such as password strength meters. Never show "invalid" while the user is still typing.
3. On blur (leaving a field): the default for format checks such as email or phone number, after the user has finished entering a value.
4. On submit: validate everything, and move focus to the first invalid field.
5. After a field has been marked invalid, re-validate on each input so the error clears as soon as it is fixed.

## Writing error messages
- Say what is wrong and how to fix it: "Enter a date after today" rather than "Invalid date".
- Be specific about the rule, not about the user: avoid "You entered an invalid email".
- Keep messages short (one sentence) and in the same position for every field (below the field).
- Do not blame: "Password must include a number" rather than "You forgot a number".
- Show the constraint before the error where possible: "Use at least 8 characters".

## Display and placement
- Inline errors appear next to the field, with an icon and text, not colour alone.
- On long forms, also show an error summary at the top with links to each invalid field.
- Mark required fields clearly; avoid asterisks as the only indicator without a legend.
- Never clear user input when an error appears.

## Accessibility
- Associate each error with its field using `aria-describedby` and set `aria-invalid="true"`.
- Announce the error summary or the first error when submission fails, via a live region or focus.
- Ensure error text meets 4.5:1 contrast.
- Provide an accessible way to recover: focus moves to the field, not just the message.

## Recovery and forgiveness
- Accept common input variations: spaces in phone numbers, uppercase or lowercase emails, pasted values with formatting.
- Format on your side when the format is unambiguous, not by rejecting input.
- Preserve entered values after a server error and show the error without wiping the form.

## Anti-patterns
Red error text appearing on every keystroke, vague messages such as "Invalid input", errors that disappear before the user can read them, clearing fields after a failed submit, and validation that only uses colour.
