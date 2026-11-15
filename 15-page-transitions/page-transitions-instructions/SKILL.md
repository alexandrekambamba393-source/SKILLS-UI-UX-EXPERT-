---
name: page-transitions-instructions
description: Expert design instructions for page and view transitions, covering continuity between screens, shared-element morphing, timing and easing, orientation of motion and accessibility safeguards such as reduced motion and focus management. Use when designing or reviewing transitions between pages, routes or major views in a web application.
---

# Page Transitions: Design Instructions

## Core principle
A transition explains the relationship between two screens. It shows where the user came from and where they are going. If a transition does not clarify a relationship, it is decoration and should be removed.

## Transition types
- Cross-fade: the default between unrelated pages. Simple and fast (200ms to 300ms).
- Shared-element: an element (thumbnail, avatar, card) morphs into its detail view. Strong continuity; use for list-to-detail navigation.
- Directional slide: forward navigation slides in from the right; back slides from the left. Reinforces the navigation stack.
- Overlay: modals and drawers rise over the current page while the page recedes slightly.

## Timing and motion
- Total transition time: 250ms to 400ms. Longer feels like waiting.
- Exit is shorter than enter (about 60 percent of the enter duration).
- Use ease-in-out for page-level motion, ease-out for entering elements.
- Keep movement within 24px to 48px for lateral slides; large motion disorients.

## Continuity and state
- Preserve scroll position on back navigation.
- Keep persistent navigation and headers stable during transitions.
- Show a loading state within the transition when data is not ready; never animate to a blank page.

## Accessibility
- Respect `prefers-reduced-motion`: replace slides and morphs with instant changes or a simple fade.
- Move focus to the new page's main heading or landmark after navigation, so keyboard and screen reader users know the page changed.
- Announce route changes in single-page apps through a live region.
- Do not block input during the transition longer than necessary.

## Browser support
- The View Transitions API provides native transitions in supporting browsers. Provide a progressive enhancement: navigation still works without it.

## Anti-patterns
Long transitions over 500ms, animations that play on every interaction, transitions that hide the destination content until the animation ends, lost focus after navigation, and transitions that ignore reduced-motion settings.
