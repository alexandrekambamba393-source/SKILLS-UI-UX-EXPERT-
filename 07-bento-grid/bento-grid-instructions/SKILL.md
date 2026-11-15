---
name: bento-grid-instructions
description: Expert design instructions for bento grid layouts, covering modular card sizing, visual hierarchy across tiles, responsive reflow and content prioritisation. Use when designing or reviewing a dashboard, product feature page or landing section built from cards of varying sizes.
---

# Bento Grid: Design Instructions

## Core principle
A bento layout groups related information into modules of different sizes, like compartments in a Japanese lunch box. Size communicates importance; proximity communicates relationship. Every tile must answer one question.

## Structure
1. Use a base grid (for example 4 or 6 columns on desktop, 2 on tablet, 1 on mobile).
2. Assign tile spans from a small set: 1x1, 2x1, 1x2, 2x2. Avoid more than four sizes in one grid.
3. Place the largest tile at the top-left or where the eye lands first, containing the primary message or hero visual.
4. Keep equal gutters (16px to 24px) and equal corner radii across all tiles.
5. Fill every cell; no unplanned holes. Plan the grid map before writing content.

## Hierarchy inside each tile
- One headline per tile, at most two sentences of supporting copy.
- Large tiles may carry a visual, metric or demo. Small tiles carry a single icon, number or label.
- Use a consistent internal padding (20px to 28px) so tiles feel like one family.

## Content priority
- Order content by importance first, then assign sizes. Do not decide sizes from aesthetics alone.
- Each tile should be scannable in under three seconds.
- Limit the grid to six to nine tiles; more becomes a wall.

## Responsive behaviour
- Desktop: multi-size composition as planned.
- Tablet: collapse to two columns; large tiles keep full width, small tiles pair up.
- Mobile: single column, ordered by priority, with the hero tile first.
- Never hide critical content on smaller screens; reorder it instead.

## Accessibility
- Reading order in the DOM must match the visual priority order.
- Interactive tiles are links or buttons with full-tile hit areas and visible focus.
- Text on gradient or image tiles must meet 4.5:1 contrast.

## Anti-patterns
Tiles of random sizes with no logic, a tile per feature with equal weight everywhere, text that overflows its tile, nested bento grids, and decorative tiles with no content.
