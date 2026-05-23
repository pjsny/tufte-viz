# ECharts: Tufte Configuration

Load this file when generating any ECharts chart code.

## Tufte theme registration

```js
const tufteTheme = {
  backgroundColor: '#fffff8',
  textStyle: {
    fontFamily: '"ET Book", "Palatino Linotype", Palatino, "Book Antiqua", Georgia, serif',
    color: '#111',
  },
  title: {
    textStyle: { fontSize: 15, fontWeight: 'normal', color: '#111' },
    left: 20,
    top: 10,
  },
  legend: { show: false },
  grid: { show: false, left: 50, right: 20, top: 50, bottom: 40 },
  categoryAxis: {
    axisLine: { lineStyle: { color: '#ccc', width: 0.5 } },
    axisTick: { show: false },
    axisLabel: { fontFamily: 'system-ui, sans-serif', fontSize: 11, color: '#999' },
    splitLine: { show: false },
  },
  valueAxis: {
    axisLine: { show: false },
    axisTick: { show: false },
    axisLabel: { fontFamily: 'system-ui, sans-serif', fontSize: 11, color: '#999' },
    splitLine: { show: false },
  },
  line: {
    lineStyle: { width: 1.5 },
    symbol: 'none',
    smooth: false,
  },
  bar: {
    barWidth: '60%',
    itemStyle: { borderRadius: [1, 1, 0, 0] },
  },
};

echarts.registerTheme('tufte', tufteTheme);
```

## Usage

```js
const chart = echarts.init(container, 'tufte');
```

## Line chart

```js
chart.setOption({
  title: { text: 'Revenue Grew 18% Year-over-Year' },
  xAxis: { type: 'category', data: labels },
  yAxis: { type: 'value' },
  series: [{
    type: 'line',
    data: values,
    lineStyle: { color: '#666', width: 1.5 },
    itemStyle: { color: '#666' },
    endLabel: {
      show: true,
      formatter: '{a}',
      fontFamily: '"ET Book", Palatino, Georgia, serif',
      fontSize: 12,
      color: '#111',
    },
    emphasis: {
      itemStyle: { borderWidth: 0 },
      lineStyle: { width: 1.5 },
    },
  }],
  tooltip: {
    trigger: 'axis',
    backgroundColor: 'transparent',
    borderWidth: 0,
    textStyle: { fontFamily: '"ET Book", Palatino, serif', fontSize: 13, color: '#111' },
    formatter: '{b}: <strong>{c}</strong>',
    axisPointer: { type: 'line', lineStyle: { color: '#ccc', width: 0.5 } },
  },
});
```

## Bar chart (horizontal)

```js
chart.setOption({
  xAxis: { type: 'value', show: false },
  yAxis: {
    type: 'category',
    data: categories,
    inverse: true,
    axisLine: { show: false },
    axisLabel: { fontFamily: '"ET Book", Palatino, serif', fontSize: 13, color: '#111' },
  },
  series: [{
    type: 'bar',
    data: values,
    itemStyle: { color: '#666' },
    label: {
      show: true,
      position: 'right',
      fontSize: 11,
      color: '#666',
    },
  }],
});
```

## Scatter plot

```js
chart.setOption({
  xAxis: { type: 'value', splitLine: { show: false } },
  yAxis: { type: 'value', splitLine: { show: false } },
  series: [{
    type: 'scatter',
    data: points,
    symbolSize: 6,
    itemStyle: { color: '#999', opacity: 0.7 },
  }],
});
```

## Direct labels via endLabel

Use `endLabel` on series instead of legends:

```js
series: [{
  name: 'Series A',
  endLabel: { show: true, formatter: '{a}', color: '#111', fontSize: 12 },
}, {
  name: 'Series B',
  endLabel: { show: true, formatter: '{a}', color: '#666', fontSize: 12 },
}]
```

## Annotations via markPoint / markLine

```js
series: [{
  markPoint: {
    data: [{ type: 'max', name: 'Peak' }],
    symbol: 'circle',
    symbolSize: 6,
    label: { formatter: 'Peak: {c}', fontSize: 11, color: '#111' },
    itemStyle: { color: '#e41a1c' },
  },
  markLine: {
    data: [{ type: 'average', name: 'Avg' }],
    lineStyle: { color: '#ccc', type: 'dashed', width: 0.5 },
    label: { formatter: 'Avg: {c}', fontSize: 10, color: '#999' },
  },
}]
```

## Dark mode

```js
const tufteDark = {
  ...tufteTheme,
  backgroundColor: '#151515',
  textStyle: { ...tufteTheme.textStyle, color: '#ddd' },
  categoryAxis: {
    ...tufteTheme.categoryAxis,
    axisLine: { lineStyle: { color: '#444', width: 0.5 } },
    axisLabel: { color: '#999' },
  },
  valueAxis: { ...tufteTheme.valueAxis, axisLabel: { color: '#999' } },
};
echarts.registerTheme('tufte-dark', tufteDark);
```
