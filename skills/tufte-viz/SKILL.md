---
name: tufte-viz
description: >-
  Design and critique data visualizations using Edward Tufte's principles.
  Use when creating, reviewing, or styling charts, graphs, dashboards,
  sparklines, or any data visualization. Applies to Recharts, ECharts,
  Chart.js, matplotlib, Plotly, D3.js, and SVG. Enforces Tufte principles
  (data-ink ratio, direct labeling, range-frame axes, small multiples)
  plus screen-first standards (accessibility, responsive, dark mode).
---

# Tufte Visualization Ideation

Apply Edward Tufte's principles to design clear, honest, high-density data visualizations.

## Workflow

### For new visualizations:

1. **Clarify the data story**
   - What comparisons matter?
   - What's the key insight to communicate?
   - Who's the audience?

2. **Select approach** using Tufte principles:
   - High comparison need -> Small multiples
   - Dense data -> Consider data tables, sparklines
   - Time-series -> Line charts with minimal grid
   - Part-to-whole -> Avoid pie charts; prefer bar/table

3. **Design with data-ink in mind**
   - Start minimal, add only what's necessary
   - Every element must earn its ink
   - Default to grayscale; use color purposefully

4. **Apply the Tufte test** (see references/tufte-principles.md)

### For critiquing visualizations:

1. **Check graphical integrity**
   - Calculate lie factor if proportions seem off
   - Verify baselines and scales
   - Look for 3D distortion

2. **Identify chartjunk**
   - Decorative elements
   - Heavy grids
   - Unnecessary 3D effects
   - Moire patterns

3. **Evaluate data-ink ratio**
   - What can be erased?
   - What's redundant?

4. **Suggest improvements** with specific before/after recommendations

### For writing chart code:

1. **Apply universal rules** from `references/implementation-guide.md`
   - 22 concrete defaults: no legends, no gridlines, range-frame axes, gray-first color, serif fonts
   - Chart type guidance, color quick reference, validation checklist

2. **Load ONE library rule file** matching the target:

   | Library | Rule file | Essential config |
   |---------|-----------|------------------|
   | Recharts | `rules/recharts.md` | `<CartesianGrid stroke="none" />`, remove `<Legend />` |
   | ECharts | `rules/echarts.md` | `splitLine: { show: false }`, `legend: { show: false }` |
   | Chart.js | `rules/chartjs.md` | `grid: { display: false }`, `plugins.legend.display: false` |
   | matplotlib | `rules/matplotlib.md` | `spines['top'].set_visible(False)`, `font.family: serif` |
   | Plotly | `rules/plotly.md` | `showgrid=False`, `plot_bgcolor='#fffff8'` |
   | D3/SVG | `rules/d3-svg.md` | `.domain { display: none }`, no background rects |

3. **Validate** against the checklist in `references/implementation-guide.md`

## Reference Files

### Theory (why)
- `references/tufte-principles.md` — core principles from *Visual Display of Quantitative Information*: lie factor, data-ink, chartjunk, small multiples, integrity.
- `references/analytical-design.md` — extensions from *Envisioning Information*, *Visual Explanations*, and *Beautiful Evidence*: the 6 principles of analytical design, sparklines, layering & separation, micro/macro, range-frames, causality, confections.

### Practice (how)
- `references/implementation-guide.md` — 22 universal rules, color/typography quick reference, chart type guidance, validation checklist.
- `references/anti-patterns.md` — detection table for common violations with per-library fix patterns.
- `rules/*.md` — library-specific code snippets and configuration (one file per library).

**Quick checklist:**
- [ ] Lie Factor ~ 1.0 (no visual distortion)
- [ ] Maximum data-ink ratio
- [ ] Zero chartjunk
- [ ] Direct labels, not legends
- [ ] Answers "compared to what?"
- [ ] Shows causality or mechanism where relevant
- [ ] Multivariate (not over-reduced)
- [ ] Words, numbers, images integrated — not segregated
- [ ] Reveals multiple levels of detail (micro + macro)
- [ ] Layering: primary data dominates, secondary recedes
- [ ] Accessible (contrast, dual encoding, aria-label)
- [ ] Responsive (adapts at narrow viewports)
- [ ] Appropriate data density
