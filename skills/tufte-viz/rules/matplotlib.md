# matplotlib: Tufte Configuration

Load this file when generating any matplotlib or seaborn chart code.

## Tufte style setup

```python
import matplotlib.pyplot as plt
import matplotlib as mpl

TUFTE = {
    'bg': '#fffff8',
    'text': '#111111',
    'text_secondary': '#666666',
    'text_tertiary': '#999999',
    'axis': '#cccccc',
    'grid': '#eeeeee',
    'series_default': '#666666',
    'highlight': '#e41a1c',
    'categorical': ['#4e79a7', '#f28e2b', '#e15759', '#76b7b2'],
    'font': 'serif',
}

plt.rcParams.update({
    'figure.facecolor': TUFTE['bg'],
    'axes.facecolor': TUFTE['bg'],
    'font.family': TUFTE['font'],
    'font.size': 12,
    'axes.labelcolor': TUFTE['text'],
    'text.color': TUFTE['text'],
    'xtick.color': TUFTE['text_tertiary'],
    'ytick.color': TUFTE['text_tertiary'],
    'axes.edgecolor': TUFTE['axis'],
    'axes.linewidth': 0.5,
    'xtick.major.size': 0,
    'ytick.major.size': 0,
    'figure.figsize': (9, 6),
})
```

## Spine removal (apply to every chart)

```python
def tufte_spines(ax, data_xrange=None, data_yrange=None):
    ax.spines['top'].set_visible(False)
    ax.spines['right'].set_visible(False)
    if data_xrange:
        ax.spines['bottom'].set_bounds(*data_xrange)
    if data_yrange:
        ax.spines['left'].set_bounds(*data_yrange)
    ax.spines['bottom'].set_linewidth(0.5)
    ax.spines['left'].set_linewidth(0.5)
```

## Line chart

```python
fig, ax = plt.subplots()
ax.plot(x, y, color=TUFTE['series_default'], linewidth=1.5)
ax.text(x[-1] + 0.5, y[-1], 'Series A', fontsize=11,
        color=TUFTE['text'], va='center')
tufte_spines(ax, data_xrange=(min(x), max(x)), data_yrange=(min(y), max(y)))
ax.set_title('Revenue Grew 18% Year-over-Year', fontsize=14, loc='left', pad=12)
```

## Bar chart

```python
fig, ax = plt.subplots()
bars = ax.barh(categories, values, color=TUFTE['series_default'], height=0.6)
for bar, val in zip(bars, values):
    ax.text(bar.get_width() + 0.5, bar.get_y() + bar.get_height()/2,
            f'{val:,.0f}', va='center', fontsize=11, color=TUFTE['text_secondary'])
ax.spines['top'].set_visible(False)
ax.spines['right'].set_visible(False)
ax.spines['bottom'].set_visible(False)
ax.tick_params(bottom=False)
ax.invert_yaxis()
```

## Scatter plot

```python
fig, ax = plt.subplots()
ax.scatter(x, y, c=TUFTE['text_tertiary'], s=20, alpha=0.7, edgecolors='none')
tufte_spines(ax)
```

## Small multiples

```python
fig, axes = plt.subplots(2, 2, figsize=(12, 8), sharex=True, sharey=True)
global_min = min(min(d) for d in all_data.values())
global_max = max(max(d) for d in all_data.values())

for ax, (cat, data) in zip(axes.flat, all_data.items()):
    ax.plot(x, data, color=TUFTE['series_default'], linewidth=1.5)
    ax.set_title(cat, fontsize=13, fontfamily='serif', loc='left')
    ax.spines['top'].set_visible(False)
    ax.spines['right'].set_visible(False)
    ax.set_ylim(global_min * 0.95, global_max * 1.05)
    ax.tick_params(labelsize=10)

fig.suptitle('Metric Comparison Across Regions', fontsize=15, x=0.08, ha='left')
plt.tight_layout()
```

## Annotations

```python
ax.annotate('Peak: $4.2M',
    xy=(peak_x, peak_y), xytext=(peak_x + 1, peak_y + 0.5),
    fontsize=11, color=TUFTE['text'],
    arrowprops=dict(arrowstyle='-', color=TUFTE['axis'], lw=0.5))
```

## Seaborn override

When using seaborn, apply Tufte defaults after setting the style:

```python
import seaborn as sns
sns.set_style('white')
sns.set_context('paper', font_scale=1.1)
sns.despine()
```
