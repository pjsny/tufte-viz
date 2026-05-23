# Anti-Patterns: Detection and Fixes

Use this reference when reviewing existing visualizations or chart code.

## Quick Reference

| Anti-pattern | Why it fails | Fix |
|---|---|---|
| **Pie chart** | Humans compare angles poorly. 28% and 32% look identical. | Horizontal bar chart, sorted by value descending |
| **3D effects** | Perspective distorts relative sizes. Back bars appear smaller. | Remove all 3D. Flat 2D only |
| **Dual y-axes** | Implies false correlation. Scale manipulation makes any two lines look related. | Two stacked charts sharing x-axis (small multiples) |
| **Legend box** | Forces eyes to shuttle between legend and data. ~40% more cognitive load. | Direct labels at endpoint or on the element |
| **Heavy gridlines** | Compete with data for attention. Create a "cage" around the data. | Remove entirely, or horizontal-only at opacity 0.08-0.12 |
| **Rainbow palette** | No natural ordering. Creates false boundaries. Inaccessible to ~8% of males. | Gray default + single accent, or 4-color muted palette |
| **Gauge/speedometer** | Enormous screen area for a single number. No trend, no context. | Large number + sparkline + comparison text |
| **Zebra-striped table** | Alternating colors create moire vibration. Stripes louder than data. | Whitespace + thin horizontal rules every 3-5 rows |
| **Gradient fills** | Add no information. Distract from values. Harder to read heights. | Solid flat color (muted gray or accent) |
| **Rotated labels** | 45/90 degree text is hard to read. Forces head-tilting. | Horizontal bar chart (flip axes), or abbreviate labels |
| **Pure white/black bg** | Harsh contrast causes eye fatigue. Looks unfinished. | `#fffff8` (light) or `#151515` (dark) |
| **Hover-only info** | Unusable on touch, invisible to screen readers, not printable. | Tap/focus fallback, or show critical values statically |
| **Color-only encoding** | Invisible to colorblind users. Fails in print/grayscale. | Add shape, pattern, or direct label alongside color |
| **Decorative animation** | Bouncing, spinning, pulsing — chartjunk in motion. | Remove, or replace with transition on data change only |
| **Truncated axes** | Exaggerates small differences by cutting off the baseline. | Show full range, or clearly mark the break |

## Per-Library Detection Patterns

### Recharts
- `<Legend />` present -> replace with direct labels via `<LabelList>` or custom `<text>`
- `<CartesianGrid />` with no stroke override -> add `stroke="none"`
- Missing `axisLine={false}` on `<YAxis>` -> top/right borders showing
- `<Pie>` component -> replace with `<Bar layout="vertical">`

### Chart.js
- `plugins.legend.display` not set to `false` -> legend showing
- `grid.display` not set to `false` -> gridlines showing
- `type: 'pie'` or `type: 'doughnut'` -> switch to horizontal bar
- `backgroundColor` with gradients -> use solid colors

### matplotlib
- `spines['top'].set_visible(True)` or not set -> add `set_visible(False)`
- `spines['right'].set_visible(True)` or not set -> add `set_visible(False)`
- `plt.legend()` called -> replace with `ax.annotate()` or `ax.text()` at data endpoints
- `plt.grid(True)` -> remove or set `alpha=0.1, axis='y'`

### Plotly
- `showlegend` not set to `False` -> legend showing
- `showgrid` not set to `False` -> gridlines showing
- `plot_bgcolor` not set -> defaults to white, use `#fffff8`
- `type='pie'` -> switch to horizontal bar

### ECharts
- `legend: { show: true }` or not set -> set `show: false`, use `endLabel`
- `splitLine: { show: true }` or not set -> set `show: false`
- `grid: { show: true }` -> set `show: false`

### D3 / SVG
- `.domain` path visible -> add `.domain { display: none }` or remove
- `<rect>` fill on chart background -> remove background rectangles
- Gridlines with full opacity -> set `stroke-opacity: 0.1` or remove
