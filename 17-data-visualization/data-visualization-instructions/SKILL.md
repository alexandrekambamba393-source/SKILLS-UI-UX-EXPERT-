---
name: data-visualization-instructions
description: Expert design instructions for data visualisation in interfaces, covering chart selection, perceptual accuracy, colour-blind-safe palettes, labelling, interactive tooltips, accessible alternatives and honest scales. Use when designing or reviewing charts, graphs, KPI indicators or other visual representations of data.
---

# Data Visualisation: Design Instructions

## Core principle
A chart exists to make a comparison, a trend or a distribution easy to see. Choose the form from the question, not from what looks impressive. Accuracy and clarity outrank decoration.

## Choosing the chart
- Comparison across categories: bar chart (horizontal when labels are long).
- Change over time: line chart; area only when the total is meaningful.
- Part of a whole: avoid pie charts beyond three segments; use stacked bars or a labelled breakdown instead.
- Distribution: histogram or box plot.
- Relationship between two measures: scatter plot.
- Single KPI: large number with a comparison (previous period, target) and a small trend indicator.

## Accuracy and honesty
1. Bar lengths start at zero. Line charts may use a non-zero baseline only when this is stated clearly.
2. Use consistent scales across related charts.
3. Sort categories by value unless a natural order (time, scale) exists.
4. Label directly where possible; avoid legends when two to three series can be labelled on the line.
5. Show units and time periods in the title or axis.

## Colour
- Use a limited palette of five to seven categorical colours maximum.
- Ensure colour is never the only encoding: pair with patterns, labels or shape markers.
- Check palettes for deuteranopia and protanopia using simulation tools.
- Meet 3:1 contrast for graphical objects against the background, and 4.5:1 for text labels.
- Use sequential palettes for ordered values and diverging palettes for values around a midpoint.

## Interaction
- Tooltips show exact values and context on hover and on keyboard focus.
- Provide a data table or text summary as an alternative for every chart.
- Animate transitions between data states briefly (250ms to 400ms) so users track changes.
- Respect reduced motion by disabling animated transitions.

## Accessibility
- Give each chart a descriptive title and a short text summary of the main takeaway.
- Use `role="img"` with an `aria-label` or an accessible description for SVG charts, or provide the data table.
- Keyboard users must be able to reach and read the data points.

## Anti-patterns
Truncated bar axes, 3D charts, rainbow palettes, pie charts with more than five slices, dual axes that suggest false correlation, and charts without units or time frames.
