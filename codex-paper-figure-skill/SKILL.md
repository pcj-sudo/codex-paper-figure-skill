---
name: codex-paper-figure-skill
description: Create editable academic-paper figures from natural-language figure descriptions, manuscript sections, methods/results text, graphical abstract ideas, or paper-specific visual concepts. Use when Codex needs to turn scientific text into a polished journal-style diagram by first generating a raster visual reference with the image generation tool, then recreating it directly as editable draw.io/.drawio mxGraphModel XML. Also use when a user asks for research figures, paper diagrams, mechanism figures, method overview figures, model architecture figures, workflow schematics, publication-ready editable diagrams, or Flaticon/icon-assisted paper figures using Codex's built-in Browser.
metadata:
  author: Pengqian Han
  version: 0.0.2
---

# Codex Paper Figure Skill

## Overview

Transform paper text or a figure concept into a publication-style editable diagram. Use image generation to explore composition and visual style, then rebuild the figure as native draw.io XML so every label, shape, connector, and icon remains editable. This skill is self-contained: it embeds the draw.io generation rules it needs instead of depending on another local skill file.

## Workflow

1. Parse the user's text into a figure brief:
   - Scientific message: the single claim or workflow the figure should communicate.
   - Entities: molecules, cells, devices, models, datasets, modules, cohorts, assays, or variables.
   - Relationships: sequence, causality, comparison, hierarchy, feedback, input/output, or spatial organization. Convert these into a canonical dependency graph before layout: every required input, distinct branch, merge, loss, feedback path, and output gets an explicit node or edge. Trace each depicted edge back to the source; do not disconnect, collapse, reverse, or invent operations for visual convenience.
   - Required labels: paper terms, abbreviations, axes, panel letters, conditions, and units that must appear verbatim.
   - Constraints: target journal style, aspect ratio, color palette, number of panels, export format, and whether the output must be editable.
   - For reproduction, treat the supplied original as the visual authority. Record its font families, weights, italics, relative type sizes, border/connector widths, arrowheads, and dash patterns before rebuilding. A generated reference must not replace these measurements.

2. Ask at most one concise clarification only when the missing detail changes the figure type or scientific meaning. Otherwise infer a sensible academic figure format from the source text.

3. Generate a raster reference with the Codex `image_gen` tool:
   - Prompt for a clean academic-paper figure, not a marketing illustration.
   - Include the exact figure structure, panel layout, visual hierarchy, palette, and domain-specific objects.
   - Request simple legible labels only when needed, but treat labels in the generated image as approximate because raster text may be imperfect.
   - Use the generated image as a composition guide only; do not deliver it as the sole final figure when the user asked for editable output.

4. Recreate the figure as draw.io:
   - Produce native `.drawio` mxGraphModel XML directly.
   - Use editable draw.io shapes, text, connectors, groups, and layers wherever possible.
   - Keep labels as real draw.io text, not embedded raster text.
   - If an exported PNG/SVG/PDF is requested, use the draw.io CLI export path below and embed the diagram XML where supported.

5. Validate before finishing:
   - Check the `.drawio` XML is well-formed and contains the required root cells.
   - Confirm all scientific entities and relationships from the brief are represented.
   - Confirm text is editable, legible, and not overlapping. Check the actual rendered fonts and bold/italic variants, not just the font names stored in XML.
   - Inspect the export at its intended publication width. For reproduction, compare it side by side with the original at the same displayed width; separately inspect typography, outlines, connectors, and icons. Structural similarity alone does not establish visual fidelity.
   - Confirm arrows and panel order match the paper logic.
   - If draw.io CLI is available, export a preview and inspect it for blank canvas, clipping, and layout issues.

## Figure Design Rules

- For new figures, prefer restrained publication styling: white or very light background, 2-4 accent colors, consistent typography, clear grouping, and generous whitespace. Use visibly substantial lettering and strokes at final display size; do not default to thin type and hairline borders.
- For reproduction, source styling takes precedence over generic design preferences, including its headings, palette, density, curved arrows, and heavier strokes. Preserve visible source labels and titles rather than applying rules intended for new figures.
- Prefer diagram-native compression over prose boxes: use domain-specific glyphs, compact repeated motifs, timelines, matrices, plots, or miniature examples for the scientific objects and reserve boxes for actual modules or boundaries. Keep labels to short terms or equations essential to the mechanism, make the main flow occupy the canvas evenly, and remove decorative callouts or dead whitespace that do not encode information.
- Use panel labels `A`, `B`, `C` only when the figure has multiple panels.
- For new figures, treat the supplied visual intent as an external caption, not canvas copy: omit a full figure title, subtitle, and caption unless the user explicitly requests them. Use only compact panel or stage headings needed to navigate the diagram.
- Prefer left-to-right or top-to-bottom reading order unless the scientific process is cyclic.
- Plan connector corridors before placing nodes. Route short orthogonal edges through whitespace, attach them to the nearest logical side, and keep every line and arrowhead outside text and node interiors; separate feedback lanes from the main flow and eliminate crossings by reordering nodes or using clearly labeled junctions.
- Encode meaning with layout first, then color; do not rely on color alone.
- Use colorblind-conscious palettes and avoid low-contrast text.
- Keep visual detail low enough that the final draw.io file is easy to edit.
- For biological figures, distinguish compartments, cells, molecules, and phenotypes with shape language and labels.
- For AI/model figures, distinguish data, model blocks, training/inference paths, losses, outputs, and evaluation with consistent containers and arrows.
- For workflow figures, use numbered steps or stage headings only when they improve scanability.

## Typography and Stroke Calibration

- Use bold lettering as the default for new diagram headings, module names, and short labels. Prefer semibold or bold for explanatory labels when final-size readability needs it. For reproduction, match the original's weight and italic treatment; avoid rebuilding a visibly bold source with regular-weight body labels.
- Changing font size does not correct insufficient font weight. Set bold explicitly (`fontStyle=1`, or `fontStyle=3` for bold italic in draw.io). Preserve italics when increasing weight. Keep mathematical subscripts and superscripts legible and editable.
- Verify that the rendering environment has the selected font and its bold/italic variants. Use a tested fallback family when needed and record the substitution; a `Times New Roman` label in XML does not prove that Times New Roman rendered. Avoid silently changing a serif source into sans serif or a typewriter label into proportional text.
- Specify separate weights for panel boundaries, module outlines, scientific connectors, broad feedback arrows, and internal icon details. Scale arrowheads along with connector widths. Maintain this hierarchy when making the figure heavier rather than applying one width to every stroke.
- Judge widths after scaling to the intended output size. For example, a 4-unit stroke on an 1800-unit canvas becomes roughly 2 pixels when displayed at 900 pixels wide. A high-resolution export can still look too thin at paper size.
- When the user says lettering or lines are too light, make a visible weight change: promote remaining regular labels to bold as appropriate and increase relevant strokes in a substantial trial step (for example, 30–50%), then inspect again. These percentages are adjustment examples, not fixed reproduction targets. Do not claim to have thickened an already-bold heading merely by reapplying its bold flag.
- Recheck wrapping, text boxes, formulas, arrow/text collisions, and clipping after changing weight or size. Keep text editable; do not simulate bold with duplicate offset labels or flatten lettering into an image. Stop once the requested adjustment is visible and the layout still works.

## Draw.io Generation Rules

- Always create a native `.drawio` file in mxGraphModel XML. Do not use Mermaid or CSV as the final editable format.
- Every diagram must include root cells:

```xml
<mxGraphModel adaptiveColors="auto">
  <root>
    <mxCell id="0"/>
    <mxCell id="1" parent="0"/>
  </root>
</mxGraphModel>
```

- Add all normal diagram elements with `parent="1"` unless deliberately using multiple layers.
- Use unique `id` values for every `mxCell`.
- Escape special characters in XML attributes: `&amp;`, `&lt;`, `&gt;`, and `&quot;`.
- Do not include XML comments in generated `.drawio` files.
- Edges must contain a child geometry element such as `<mxGeometry relative="1" as="geometry"/>`.
- Use descriptive lowercase hyphenated filenames, for example `cell-state-pipeline.drawio`.
- If the user requests PNG, SVG, or PDF, first create the `.drawio` file, then export with draw.io CLI using embedded diagram XML when available.
- If the draw.io CLI is unavailable, an available official draw.io browser renderer may export the native diagram instead. Inspect that export and state which renderer was used. If neither is available, keep the `.drawio` file and report that preview/export was skipped. Do not claim a CLI export when another renderer produced the artifact.

Common draw.io CLI locations:

- Windows: `C:\Program Files\draw.io\draw.io.exe`
- macOS: `/Applications/draw.io.app/Contents/MacOS/draw.io`
- Linux: `drawio` on `PATH`

Export command pattern:

```bash
drawio -x -f <png|svg|pdf> -e -b 10 -o <output> <input.drawio>
```

## Icon Workflow

Use icons only when they improve recognition or reduce clutter. If icons are needed, use Codex's built-in `Browser` plugin as the default Flaticon workflow so users do not need extra tools.

1. Use `Browser` to open `https://www.flaticon.com/` and search the exact concept plus a style term such as `line`, `outline`, `filled`, `flat`, or `science`.
2. Inspect candidate results in the browser. Prefer free, non-premium icons from one style family or author so the figure feels coherent.
3. Open the icon detail page and extract:
   - Icon page URL.
   - Icon title or concept.
   - Designer/author.
   - License and attribution requirement visible on the page.
   - Download URL or CDN image URL, if available.
4. Download the icon only after the license and attribution requirement are clear:
   - Prefer SVG/vector when available and permitted.
   - Use the browser download flow when the current Browser runtime can save the file.
   - If Browser can verify the icon page but cannot directly save the download, extract the official image/CDN URL and download it locally with a normal HTTP request.
   - Save project-bound icon files under the current figure output folder, preferably `icons/`, with descriptive lowercase filenames.
5. Avoid premium, locked, login-required, or unclear-license icons. If licensing is not usable or the icon cannot be downloaded reliably, create a simple editable draw.io shape instead.
6. In draw.io, keep each icon as a distinct movable image element. Do not flatten icons into a background image.
7. Record icon source URLs, author, and attribution/license notes in the final response and any README/example output that uses external icons.

## Output Contract

Deliver the editable `.drawio` file unless the user asks for another format. When helpful, also provide:

- The generated raster reference image path.
- An exported preview path such as `.drawio.png` or `.drawio.svg`.
- A short note listing assumptions, icon sources, and any unresolved fidelity limits.

Never present a raster-only image as complete when the user requested an editable academic figure.
