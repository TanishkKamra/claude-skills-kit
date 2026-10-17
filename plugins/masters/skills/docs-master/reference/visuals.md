# Visuals

Show the master's **Visual aids** at these moments (unless the user chose Text only):
- **Brief**: a picture of what will change before the user says go: files or areas touched, the flow, a before/after sketch.
- **Decisions** (Full only): options side by side (directions, approaches, panel proposals with the judge's scores).
- **Report**: proof of what changed: before/after screenshots, a diff map, charts of test or metric results.
- **Live board** (only if the user said yes): done / running / next, redrawn after each step.

## Where
1. **Inline in the chat**, best tool first: an inline visual/widget tool (load its guide first if it has one) → an Artifact → a Mermaid diagram or a small Markdown table.
2. **The run's report page**: copy `visuals/report-template.html` from this skill's folder to the run folder as `report.html`, fill its sections as the run goes, and open it for the user (browser pane, Artifact, or give the path). On claude.ai, publish it as an Artifact. If the template is missing, use a Markdown file and say so.

A master's "Component" column names a block in the template: `flow` is `<template id="c-flow">`, likewise `before-after`, `diff-bars`, `options`, `table`, `heatmap`, `timeline`, `swatches`, `board`, `diagram`. Copy the template's inner HTML into the right section and fill it with real data.

## Rules
- Real data only: numbers, files and screenshots come from the repo, the diff, test output or the browser. Label anything illustrative "sketch".
- The title states the takeaway ("3 files change, no public API touched").
- The template's accent color for emphasis, gray for everything else, red only for problems, green only for passes.
- Every visual has a one-line text equivalent.
- UI screenshots at 1440px and 390px widths.
