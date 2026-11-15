---
name: command-palette-instructions
description: Expert design instructions for command palettes (for example Ctrl or Cmd plus K), covering command discovery, fuzzy search and ranking, recent items, keyboard-first operation, accessibility of combobox patterns and discoverability alongside visible navigation. Use when designing or reviewing a keyboard-driven command or search overlay for power users in web or desktop applications.
---

# Command Palette: Design Instructions

## Core principle
A command palette lets users reach any action or destination by typing its name. It accelerates expert use and makes large products navigable. It must never replace visible navigation: every command should also be reachable through the interface.

## Purpose and scope
- Use for products with many actions and destinations, such as editors, dashboards, developer tools and admin consoles.
- Include navigation (go to a page), actions (create, export), and recent or frequent items.
- Do not use it as the only way to find core features for occasional users.

## Opening and closing
- Open with a conventional shortcut (Cmd or Ctrl plus K is widely recognised) and show the shortcut in the interface and in help.
- Close with Escape, and on selection. Return focus to the element that was focused before opening.
- Opening must be fast: under 100ms, with the input focused immediately.

## Search and ranking
1. Match on command name, synonyms and keywords (for example "delete", "remove", "trash").
2. Use fuzzy matching but rank exact and prefix matches first.
3. Show recent and frequent commands when the input is empty.
4. Group results by type (Actions, Pages, Recent) with clear headings.
5. Show the keyboard shortcut next to each command where one exists.
6. Keep the result list to about seven to ten visible items, with scrolling for more.

## Keyboard behaviour
- Arrow Up and Down move the active item; Enter runs it.
- Home and End move to the first and last items when focus is in the input.
- Tab should not escape the palette while it is open; focus stays inside.
- Show an empty state with a suggestion when nothing matches.

## Accessibility
- Implement the ARIA combobox pattern: the input has `role="combobox"`, `aria-expanded`, `aria-controls` and `aria-activedescendant`.
- The results list uses `role="listbox"` with `role="option"` items and `aria-selected` on the active item.
- Announce result counts through a live region.
- Ensure the active item has a visible highlight that meets 3:1 contrast.

## Design
- Use a centred overlay with a backdrop that dims the page.
- Keep the input prominent and the list readable at 15px to 16px.
- Use consistent icons and keep text labels for all commands.

## Anti-patterns
Palettes that are the only way to reach core features, commands with unclear names, no empty-state guidance, losing focus when results update, and ignoring keyboard users in the design.
