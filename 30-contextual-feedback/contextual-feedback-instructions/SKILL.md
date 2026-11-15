---
name: contextual-feedback-instructions
description: Expert design instructions for contextual feedback, covering success, error, warning and info messages, toasts, inline messages, banners and confirmations, with guidance on placement, duration, wording, persistence and accessible announcement. Use when designing or reviewing how an interface tells users the outcome of their actions or the state of the system.
---

# Contextual Feedback: Design Instructions

## Core principle
Feedback tells the user what happened and what to do next, close to where they acted. The message type, placement and duration should match the importance and reversibility of the event.

## Choosing the feedback type
- Inline message: tied to a field or section (form validation, field-level error). Use for issues the user must correct in place.
- Toast or snackbar: brief confirmation of a completed, reversible action (saved, moved to trash). Auto-dismiss after 4 to 8 seconds, and offer undo where possible.
- Banner: persistent, page-level status that affects many items (service degraded, unsaved changes, trial ending). Stays until resolved or dismissed.
- Modal confirmation: only for irreversible or high-impact actions (permanent deletion, payment, sending to an external audience).
- Status indicator: ongoing or background state (syncing, offline, last saved at 14:02).

## Message types
- Success: confirm what was done ("Project saved"), with a short follow-up if relevant.
- Error: explain what failed, why in plain language if known, and how to fix or retry. Never blame the user.
- Warning: a risk the user should consider before continuing.
- Info: neutral information that is useful but not urgent.
- Colour supports the message type but never carries meaning alone; always include an icon and text.

## Wording
- Be specific: "Invoice sent to 3 recipients" rather than "Success".
- Lead with the outcome, then the next step: "Upload failed. Check your connection and try again."
- Keep messages to one or two short sentences.
- Use the same verbs across the product: "Save" always saves, "Remove" always removes from view.

## Timing and persistence
- Success toasts disappear automatically, but errors that require action persist until resolved.
- Do not stack many toasts; collapse or queue them.
- Pause auto-dismiss while the user is hovering or focused on the message.

## Accessibility
- Use `role="status"` with `aria-live="polite"` for success and info messages.
- Use `role="alert"` or `aria-live="assertive"` only for urgent errors that require immediate attention.
- Keep focus in place for toasts; move focus to the first error for form submission failures.
- Dismiss buttons must be labelled and keyboard reachable.
- Messages must not disappear before a typical user can read them (aim for at least 5 seconds for short text, longer for more).

## Anti-patterns
Generic "Something went wrong" with no next step, toasts that cover primary actions, errors shown only in colour, success messages for every trivial action, auto-dismissing errors that need action, and modal dialogs for simple confirmations.
