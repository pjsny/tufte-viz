# Chart.js: Tufte Configuration

Load this file when generating any Chart.js chart code.

## Tufte defaults (register once)

```js
const TUFTE = {
  bg: '#fffff8',
  text: '#111',
  textSecondary: '#666',
  textTertiary: '#999',
  axis: '#ccc',
  grid: '#eee',
  seriesDefault: '#666',
  highlight: '#e41a1c',
  categorical: ['#4e79a7', '#f28e2b', '#e15759', '#76b7b2'],
  font: '"ET Book", "Palatino Linotype", Palatino, "Book Antiqua", Georgia, serif',
  fontSans: 'system-ui, -apple-system, sans-serif',
};

Chart.defaults.font.family = TUFTE.fontSans;
Chart.defaults.font.size = 12;
Chart.defaults.color = TUFTE.textTertiary;
Chart.defaults.plugins.legend.display = false;
Chart.defaults.plugins.tooltip.backgroundColor = 'transparent';
Chart.defaults.plugins.tooltip.titleColor = TUFTE.textSecondary;
Chart.defaults.plugins.tooltip.bodyColor = TUFTE.text;
Chart.defaults.plugins.tooltip.borderWidth = 0;
Chart.defaults.plugins.tooltip.padding = 4;
Chart.defaults.plugins.tooltip.displayColors = false;
```

## Line chart

```js
new Chart(ctx, {
  type: 'line',
  data: {
    labels: labels,
    datasets: [{
      data: values,
      borderColor: TUFTE.seriesDefault,
      borderWidth: 1.5,
      pointRadius: 0,
      pointHoverRadius: 3,
      pointHoverBackgroundColor: TUFTE.text,
      tension: 0,
      fill: false,
    }],
  },
  options: {
    responsive: true,
    aspectRatio: 1.5,
    plugins: {
      legend: { display: false },
      datalabels: {
        display: (ctx) => ctx.dataIndex === ctx.dataset.data.length - 1,
        anchor: 'end', align: 'right',
        font: { family: TUFTE.font, size: 12 },
        color: TUFTE.text,
      },
    },
    scales: {
      x: {
        grid: { display: false },
        border: { display: true, color: TUFTE.axis, width: 0.5 },
        ticks: { padding: 8 },
      },
      y: {
        grid: { display: false },
        border: { display: false },
        ticks: { padding: 4 },
      },
    },
  },
});
```

## Bar chart (horizontal)

```js
new Chart(ctx, {
  type: 'bar',
  data: {
    labels: categories,
    datasets: [{
      data: values,
      backgroundColor: TUFTE.seriesDefault,
      borderRadius: 1,
      barPercentage: 0.6,
    }],
  },
  options: {
    indexAxis: 'y',
    responsive: true,
    aspectRatio: 1.5,
    plugins: {
      legend: { display: false },
      datalabels: {
        anchor: 'end', align: 'end',
        font: { size: 11 },
        color: TUFTE.textSecondary,
      },
    },
    scales: {
      x: {
        grid: { display: false },
        border: { display: false },
        ticks: { display: false },
      },
      y: {
        grid: { display: false },
        border: { display: false },
        ticks: { font: { family: TUFTE.font, size: 13 }, color: TUFTE.text },
      },
    },
  },
});
```

## Scatter plot

```js
new Chart(ctx, {
  type: 'scatter',
  data: {
    datasets: [{
      data: points,
      backgroundColor: TUFTE.textTertiary,
      pointRadius: 3,
      pointHoverRadius: 5,
    }],
  },
  options: {
    responsive: true,
    aspectRatio: 1.5,
    scales: {
      x: { grid: { display: false }, border: { color: TUFTE.axis, width: 0.5 } },
      y: { grid: { display: false }, border: { display: false } },
    },
  },
});
```

## Plugin: chartjs-plugin-datalabels

Use for direct labels instead of legends. Install separately:

```html
<script src="https://cdn.jsdelivr.net/npm/chartjs-plugin-datalabels@2"></script>
```

```js
Chart.register(ChartDataLabels);
```

## Dark mode

```js
function applyDarkMode() {
  Chart.defaults.color = '#999';
  Chart.defaults.plugins.tooltip.titleColor = '#999';
  Chart.defaults.plugins.tooltip.bodyColor = '#ddd';
  // Update canvas container background to #151515
}
```
