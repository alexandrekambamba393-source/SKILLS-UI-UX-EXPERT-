---
name: intuitive-navigation-instructions
description: Expert design instructions for intuitive navigation, covering information architecture, wayfinding, menu structure, breadcrumbs, search, consistent placement and mobile navigation patterns. Use when designing or reviewing site and app navigation structures so users can find content and features quickly.
---

# Intuitive Navigation: Design Instructions

## Core principle
Users should always know where they are, where they can go, and how to get back. Navigation is an information architecture problem first and a visual problem second. A clear structure beats clever menus.

## Information architecture
1. Start from user tasks and mental models. Use card sorting and tree testing before building menus.
2. Limit top-level items to five to seven. More than that overloads choice.
3. Name items with the users' vocabulary, not internal jargon. Labels should be specific and mutually exclusive.
4. Keep depth shallow: most content should be reachable in three clicks or fewer.
5. Group related items under clear parents; avoid orphan pages.

## Wayfinding
- Highlight the current location in the primary navigation (aria-current="page").
- Use breadcrumbs for hierarchies deeper than two levels, and show the path from the top down.
- Keep the logo or home link in the same place on every page.
- Provide a clear page title that matches the navigation label that led there.

## Consistency
- Navigation placement, order and labels stay identical across all pages.
- Global actions (search, account, help) sit in predictable locations.
- Use the same pattern for similar content types (for example all lists use the same filter bar).

## Search and shortcuts
- Offer search for any site with more than about 50 pages or any app with significant content.
- Provide autocomplete and recent items, and show helpful empty results with alternative paths.
- Keyboard shortcuts (for example command palette) are an enhancement, never the only route.

## Mobile navigation
- Use a bottom navigation bar for three to five top-level destinations in apps.
- Use a hamburger menu only when the number of destinations exceeds that, and label it "Menu" to avoid ambiguity.
- Keep primary actions within thumb reach.

## Accessibility
- Wrap primary navigation in a `nav` landmark with an accessible name when there is more than one nav.
- Skip links must be the first focusable element.
- Dropdown menus must be operable by keyboard and must not close unexpectedly on focus change.
- Touch targets at least 44px.

## Anti-patterns
Mega menus with dozens of links, hidden navigation that requires hover, inconsistent labels between menu and page title, icon-only navigation without labels, and search results with no way to refine.
