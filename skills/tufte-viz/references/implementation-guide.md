# Implementation Guide: Tufte Principles on Screen

Translates the theory in `tufte-principles.md` and `analytical-design.md` into concrete, actionable rules for building data visualizations in code.

---

## Universal Rules

Apply these defaults to every chart. Deviate only when the user explicitly requests otherwise.

### Structure

1. **Remove top and right borders.** Bottom and left axes are sufficient. Top and right lines are pure chartjunk.

2. **Direct labels, not legends.** Label each data series at its endpoint or on the element itself. Remove legend components entirely. If there is only one series, the chart title provides context.

3. **No gridlines by default.** If precise value reading is needed, add horizontal-only gridlines at very low opacity (0.08-0.12). For interactive charts, prefer a contextual crosshair on hover instead.

4. **Range-frame axes.** Axis lines span only the range of the data, not from zero to some arbitrary maximum. Start at (or near) the minimum data value, end at the maximum.

5. **No 3D effects.** No perspective, depth, or shadows on chart elements. 2D data gets 2D representation.

6. **No pie charts unless explicitly requested.** Default to horizontal bar chart sorted by value. If forced: max 4 slices, 2D only, start at 12 o'clock, direct percentage labels.

### Visual Design

7. **Aspect ratio ~1.5:1.** Charts should be approximately 50% wider than tall. Standard sizes: 600x400, 750x500, 900x600.

8. **Gray first, highlight selectively.** Default series color: medium gray (`#666`). Use a single accent color for the most important series or point. Max 4 distinct colors. Choose palette type by data: categorical (unordered groups), sequential (ordered magnitude), diverging (deviation from midpoint).

9. **Off-white background.** Light mode: `#fffff8`. Dark mode: `#151515`. Never use pure white or pure black.

10. **Serif fonts for data.** Use `"ET Book", "Palatino Linotype", Palatino, "Book Antiqua", Georgia, serif` for labels, annotations, titles. Sans-serif is acceptable only for small axis tick labels (11-12px).

11. **No dual y-axes.** Two y-axes imply false correlations. Use small multiples instead — two charts stacked vertically with shared x-axis.

### Content

12. **Annotate the notable.** If the data contains a peak, trough, inflection, or event boundary, add a text annotation directly on the chart. Use a short leader line if needed for spacing.

13. **Show comparison context.** Include at least one reference element: a reference line (average, target, prior period), a shaded band, or a second series. A chart with no context fails the "Compared to what?" test.

14. **Titles assert findings.** "Revenue Surged 23% in Q3" not "Revenue by Quarter, 2024". Subtitles provide context.

15. **Format numbers for humans.** $1.2M not $1,200,000. Use thousand separators for mid-range. Match decimal precision to significance. Right-align numbers in tables.

16. **Don't chart what a sentence can say.** 1-2 numbers? Write a sentence. Simple ranking of 3-5 items? Use a table. Charts earn their space by revealing patterns text cannot.

### Screen-First

17. **Progressive disclosure.** Default to Tufte-clean overview. Layer details through hover, tap, click. Don't frontload everything onto a single static view.

18. **Accessible by default.** 3:1 contrast minimum for chart elements; 4.5:1 for text. Never use color as sole differentiator — pair with shape, pattern, or label. Provide `aria-label` with key finding for every chart. Keyboard-navigable interactive charts.

19. **Responsive layout.** Charts must adapt — fluid (percentage width + viewBox), adaptive (breakpoint layout changes), or hybrid. At narrow viewports, change chart type or layout, don't just shrink.

20. **Animate to explain, not decorate.** Transitions for data changes (sorting, filtering) are useful. Entrance animations, bouncing, pulsing are chartjunk. Duration: 200-500ms, ease-out. Respect `prefers-reduced-motion`.

21. **Dark mode as first-class.** Design both palettes intentionally. Reduce saturation in dark mode. Use semantic color tokens so charts adapt automatically.

22. **Minimal tooltips.** Plain text with data value and label. No colored background, border, arrow, or shadow.

---

## Color Quick Reference

| Token | Light | Dark |
|---|---|---|
| Background | `#fffff8` | `#151515` |
| Text primary | `#111` | `#ddd` |
| Text secondary | `#666` | `#999` |
| Axis/rule | `#ccc` | `#444` |
| Grid (if used) | `#eee` at 8-12% opacity | `#333` |
| Default series | `#666` | `#999` |
| Highlight | `#e41a1c` | `#fc8d62` |

**Categorical (max 4):** `#4e79a7` steel blue, `#f28e2b` tangerine, `#e15759` coral, `#76b7b2` sage

**Font stack:** `"ET Book", "Palatino Linotype", Palatino, "Book Antiqua", Georgia, serif`
**Sans (ticks only):** `system-ui, -apple-system, sans-serif`

---

## Chart Type Guidance

| Type | Key settings |
|---|---|
| **Line** | 1.5-2px stroke, no dots unless <7 points (then r=2), direct label at rightmost point |
| **Bar** | Prefer horizontal for categories, sort by value descending, direct value labels, `#7a7a7a` default fill |
| **Scatter** | Gray dots `#999` r=3, highlight key cluster/outlier with accent, regression line if meaningful (dashed, thin) |
| **Time series** | Label events on chart, range-frame x-axis, YoY via opacity (current solid, prior 30%) |
| **Small multiples** | Same scale ALL panels, shared axis labels (x on bottom row, y on left column), no panel borders |
| **Sparklines** | ~80x20px, no axes/labels/gridlines, min/max dots r=1.5, embed inline |
| **Data tables** | No zebra striping, whitespace + thin rules every 3-5 rows, right-align numbers |
| **Slopegraph** | Before/after categories, label both endpoints, gray default + highlight key slopes |
| **Area** | Prefer lines. If area: fillOpacity 0.03-0.08, no gradient, direct labels at endpoints |
| **Heatmap** | Sequential or diverging palette only, value labels in cells, companion data table for accessibility |

---

## Validation Checklist

Before presenting any chart:

- [ ] No top or right borders/spines
- [ ] No legend — series labeled directly on chart
- [ ] Gridlines removed or horizontal-only at opacity <= 0.12
- [ ] Aspect ratio ~1.5:1
- [ ] Background `#fffff8` (light) or `#151515` (dark)
- [ ] Serif font for labels and titles
- [ ] Default series gray; color only for emphasis
- [ ] No 3D, no pie chart (unless requested)
- [ ] Axes span only the data range (range-frame)
- [ ] Notable features annotated directly on chart
- [ ] Comparison context present (reference line, band, or second series)
- [ ] Tooltips plain text, no decoration
- [ ] Interactive elements have tap/click/focus alternatives
- [ ] Contrast meets 3:1 (elements) / 4.5:1 (text)
- [ ] Chart has aria-label or text alternative
- [ ] Animations respect `prefers-reduced-motion`
- [ ] Renders usably at 320px and 1440px+
- [ ] Title states the finding, not the axis description
- [ ] Numbers formatted for readability
- [ ] Chart is warranted — data couldn't be a sentence or table
