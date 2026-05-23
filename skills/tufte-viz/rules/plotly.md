# Plotly: Tufte Configuration

Load this file when generating any Plotly (Python or JS) chart code.

## Tufte layout template

```python
import plotly.graph_objects as go

tufte_layout = dict(
    plot_bgcolor='#fffff8',
    paper_bgcolor='#fffff8',
    font=dict(family='Palatino, "Book Antiqua", Georgia, serif', size=13, color='#111'),
    margin=dict(l=50, r=20, t=50, b=40),
    width=750,
    height=500,
    showlegend=False,
    xaxis=dict(
        showgrid=False,
        zeroline=False,
        linecolor='#ccc',
        linewidth=0.5,
        tickfont=dict(family='system-ui, sans-serif', size=11, color='#999'),
    ),
    yaxis=dict(
        showgrid=False,
        zeroline=False,
        showline=False,
        tickfont=dict(family='system-ui, sans-serif', size=11, color='#999'),
    ),
)
```

## Line chart

```python
fig = go.Figure()
fig.add_trace(go.Scatter(
    x=x, y=y,
    mode='lines',
    line=dict(color='#666', width=1.5),
    hovertemplate='%{x}: %{y}<extra></extra>',
))
fig.add_annotation(
    x=x[-1], y=y[-1], text='Series A',
    showarrow=False, xanchor='left', xshift=8,
    font=dict(size=12, color='#111'),
)
fig.update_layout(**tufte_layout)
fig.update_layout(title=dict(
    text='Revenue Grew 18% Year-over-Year',
    x=0.05, xanchor='left', font=dict(size=15),
))
```

## Bar chart (horizontal)

```python
fig = go.Figure()
fig.add_trace(go.Bar(
    y=categories, x=values,
    orientation='h',
    marker_color='#666',
    text=values, textposition='outside',
    textfont=dict(size=11, color='#666'),
))
fig.update_layout(**tufte_layout)
fig.update_layout(
    yaxis=dict(autorange='reversed', showline=False),
    xaxis=dict(showline=False, showticklabels=False),
)
```

## Scatter plot

```python
fig = go.Figure()
fig.add_trace(go.Scatter(
    x=x, y=y,
    mode='markers',
    marker=dict(color='#999', size=6, opacity=0.7),
    hovertemplate='(%{x}, %{y})<extra></extra>',
))
fig.update_layout(**tufte_layout)
```

## Small multiples with subplots

```python
from plotly.subplots import make_subplots

fig = make_subplots(rows=2, cols=2, subplot_titles=categories,
                     shared_xaxes=True, shared_yaxes=True,
                     vertical_spacing=0.08, horizontal_spacing=0.06)

for i, (cat, data) in enumerate(all_data.items()):
    row, col = i // 2 + 1, i % 2 + 1
    fig.add_trace(go.Scatter(
        x=x, y=data, mode='lines',
        line=dict(color='#666', width=1.5),
        showlegend=False,
    ), row=row, col=col)

fig.update_layout(**tufte_layout)
fig.update_annotations(font=dict(size=13, family='Palatino, serif'))
```

## Annotations

```python
fig.add_annotation(
    x=peak_x, y=peak_y,
    text='Peak: $4.2M',
    showarrow=True, arrowhead=0, arrowwidth=0.5, arrowcolor='#ccc',
    ax=40, ay=-30,
    font=dict(size=11, color='#111'),
)
```

## Plotly Express shorthand

```python
import plotly.express as px

fig = px.line(df, x='date', y='value',
              template='simple_white')
fig.update_layout(**tufte_layout)
fig.update_traces(line_color='#666', line_width=1.5)
```

## Dark mode

```python
tufte_dark = {**tufte_layout,
    'plot_bgcolor': '#151515',
    'paper_bgcolor': '#151515',
    'font': dict(family='Palatino, serif', size=13, color='#ddd'),
    'xaxis': {**tufte_layout['xaxis'], 'linecolor': '#444',
              'tickfont': dict(color='#999')},
    'yaxis': {**tufte_layout['yaxis'], 'tickfont': dict(color='#999')},
}
```
