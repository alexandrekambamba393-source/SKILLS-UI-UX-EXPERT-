---
name: design-system-instructions
description: Expert design instructions for building and governing a design system, covering foundations (tokens for colour, type, spacing and motion), component principles, naming conventions, theming, documentation, versioning and adoption. Use when establishing, extending or auditing a shared design system for a product or organisation.
---

# Design System: Design Instructions

## Core principle
A design system is a shared language of decisions. It turns repeated choices into reusable tokens and components so that teams build consistently and faster. It is a product with users, maintainers and a roadmap, not a folder of files.

## Layers
1. Foundations (tokens): colour, typography, spacing, radius, elevation, motion and breakpoints, expressed as named values.
2. Components: buttons, inputs, cards, navigation and feedback elements, built from tokens.
3. Patterns: combinations of components for common tasks (forms, empty states, data tables).
4. Documentation: usage guidance, do and don't examples, accessibility notes and code snippets.

## Token architecture
- Use three tiers: primitive (raw values such as blue-600), semantic (intent such as color-action-primary), and component (for example button-background).
- Components reference semantic tokens only, never primitives. This makes theming possible.
- Name tokens by purpose, not appearance: "color-text-muted" rather than "grey-500".
- Keep spacing on a consistent scale (for example 4px base with steps of 4, 8, 12, 16, 24, 32, 48, 64).

## Component principles
- Each component has a clear purpose, a documented API and a set of states (default, hover, focus, active, disabled, loading, error).
- Accessibility is built in: keyboard, focus, labels, roles and contrast are part of the component specification.
- Prefer composition over configuration. Small primitives combine into larger patterns.
- Avoid one-off variants. A new variant is added only when it is used in several places.

## Theming and modes
- Theme by swapping semantic token values (light, dark, brand variants) rather than rewriting components.
- Verify contrast for every theme.

## Governance
- Establish an owner or small core team and a contribution process.
- Version the system with semantic versioning; document breaking changes and migration steps.
- Deprecate components with notice, a replacement and a removal date.
- Track adoption and gather feedback from consuming teams.

## Documentation standards
- Every component page includes purpose, anatomy, variants, states, accessibility, usage guidance and code.
- Show do and don't examples with clear reasons.
- Keep examples current with the latest version.

## Anti-patterns
Components that reference raw colour values, duplicated components with small differences, undocumented variants, theming by overriding styles in each component, and systems that are never updated after launch.
