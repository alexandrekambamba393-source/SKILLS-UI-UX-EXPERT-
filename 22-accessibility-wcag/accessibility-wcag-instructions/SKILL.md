---
name: accessibility-wcag-instructions
description: Expert design instructions for accessibility under WCAG 2.2 Level AA, covering perceivable, operable, understandable and robust principles, contrast thresholds, focus appearance, target size, keyboard operation, cognitive load and accessible design process. Use when designing, auditing or reviewing any interface for accessibility compliance.
---

# Accessibility (WCAG 2.2): Design Instructions

## Core principle
Accessibility is a baseline quality, not a feature. The interface must work for people using screen readers, keyboards, voice control, magnification, reduced motion and high-contrast settings. Build it in from the first sketch.

## The four principles (POUR)
1. Perceivable: information is available to more than one sense; text alternatives exist for non-text content.
2. Operable: every function works from a keyboard; users have enough time; nothing causes seizures; navigation is findable.
3. Understandable: language is clear; behaviour is predictable; errors are explained and fixable.
4. Robust: markup is valid and compatible with assistive technology.

## Key WCAG 2.2 AA requirements for designers
- Contrast: body text 4.5:1, large text (24px regular or 18.66px bold) 3:1, UI components and graphical objects 3:1 (1.4.11).
- Reflow: content works at 320px width without horizontal scrolling (1.4.10).
- Text spacing: layout holds when line-height, paragraph, letter and word spacing are increased.
- Focus appearance (2.4.11, 2.4.13): focus indicators are visible, at least 2px thick with 3:1 contrast against adjacent colours, and not obscured by sticky headers.
- Target size minimum (2.5.8): 24px by 24px, with spacing rules; design to 44px for touch as best practice.
- Dragging movements (2.5.7): every drag action has a single-pointer alternative.
- Accessible authentication (3.3.8): do not require cognitive function tests such as memorising or transcribing; allow paste and password managers.
- Redundant entry (3.3.7): do not ask users to re-enter information already provided in the same process.
- Consistent help (3.2.6): help is in the same place across pages.

## Design checklist
- Every colour pair meets contrast for its role, including hover, disabled and error states.
- Colour is never the only indicator of state.
- Every image has an alternative text decision: informative (describe), decorative (empty), or complex (long description).
- Forms have visible labels, clear instructions, and error messages that identify the field and suggest a fix.
- Motion can be paused, and nothing flashes more than three times per second.
- Headings form a logical outline; landmarks define page regions.

## Process
- Include accessibility in design reviews, not only in QA.
- Test with keyboard only, a screen reader, browser zoom at 200 percent and reduced-motion settings.
- Document decisions in the design system so components stay accessible when reused.

## Anti-patterns
Placeholder text used as labels, icon-only buttons with no name, focus removed with outline none, custom controls that break keyboard use, color-only error states, and images of text.
