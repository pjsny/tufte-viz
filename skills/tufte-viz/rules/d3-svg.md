# D3.js / SVG / HTML: Tufte Configuration

Load this file when generating D3.js, raw SVG, or HTML-based chart code.

## Tufte constants

```js
const TUFTE = {
  bg: '#fffff8',
  text: '#111',
  textSecondary: '#666',
  textTertiary: '#999',
  axis: '#ccc',
  seriesDefault: '#666',
  highlight: '#e41a1c',
  categorical: ['#4e79a7', '#f28e2b', '#e15759', '#76b7b2'],
  font: '"ET Book", "Palatino Linotype", Palatino, "Book Antiqua", Georgia, serif',
  fontSans: 'system-ui, -apple-system, sans-serif',
};
```

## CSS defaults

```css
.tufte-chart {
  background: #fffff8;
  font-family: "ET Book", "Palatino Linotype", Palatino, "Book Antiqua", Georgia, serif;
}
.tufte-chart .domain { display: none; }
.tufte-chart .tick line { stroke: #ccc; stroke-width: 0.5; }
.tufte-chart text { fill: #999; font-family: system-ui, sans-serif; font-size: 11px; }
.tufte-chart .annotation { fill: #111; font-family: "ET Book", Palatino, serif; font-size: 12px; }
.tufte-chart .series { stroke: #666; stroke-width: 1.5; fill: none; }
```

## D3 axis setup

```js
const margin = { top: 30, right: 60, bottom: 40, left: 50 };
const width = 750 - margin.left - margin.right;
const height = 500 - margin.top - margin.bottom;

const svg = d3.select(container)
  .append('svg')
  .attr('viewBox', `0 0 750 500`)
  .attr('class', 'tufte-chart')
  .append('g')
  .attr('transform', `translate(${margin.left},${margin.top})`);

const xScale = d3.scaleLinear().domain(d3.extent(data, d => d.x)).range([0, width]);
const yScale = d3.scaleLinear().domain(d3.extent(data, d => d.y)).range([height, 0]);

// X axis — bottom only, range-frame
svg.append('g')
  .attr('transform', `translate(0,${height})`)
  .call(d3.axisBottom(xScale).ticks(6))
  .call(g => g.select('.domain').remove())
  .call(g => g.selectAll('.tick line').attr('stroke', '#ccc').attr('stroke-width', 0.5));

// Y axis — no line, ticks only
svg.append('g')
  .call(d3.axisLeft(yScale).ticks(5))
  .call(g => g.select('.domain').remove())
  .call(g => g.selectAll('.tick line').remove());
```

## Line chart

```js
const line = d3.line()
  .x(d => xScale(d.x))
  .y(d => yScale(d.y));

svg.append('path')
  .datum(data)
  .attr('class', 'series')
  .attr('d', line);

// Direct label at endpoint
const last = data[data.length - 1];
svg.append('text')
  .attr('class', 'annotation')
  .attr('x', xScale(last.x) + 8)
  .attr('y', yScale(last.y))
  .attr('dy', '0.35em')
  .text('Series A');
```

## Bar chart (horizontal)

```js
const yBand = d3.scaleBand()
  .domain(data.map(d => d.category))
  .range([0, height])
  .padding(0.35);

svg.selectAll('rect')
  .data(data)
  .join('rect')
  .attr('y', d => yBand(d.category))
  .attr('width', d => xScale(d.value))
  .attr('height', yBand.bandwidth())
  .attr('fill', TUFTE.seriesDefault)
  .attr('rx', 1);

// Direct value labels
svg.selectAll('.value-label')
  .data(data)
  .join('text')
  .attr('class', 'annotation')
  .attr('x', d => xScale(d.value) + 6)
  .attr('y', d => yBand(d.category) + yBand.bandwidth() / 2)
  .attr('dy', '0.35em')
  .attr('font-size', 11)
  .attr('fill', TUFTE.textSecondary)
  .text(d => d.value.toLocaleString());
```

## Scatter plot

```js
svg.selectAll('circle')
  .data(data)
  .join('circle')
  .attr('cx', d => xScale(d.x))
  .attr('cy', d => yScale(d.y))
  .attr('r', 3)
  .attr('fill', TUFTE.textTertiary)
  .attr('opacity', 0.7);
```

## Inline SVG sparkline

For embedding in tables or prose:

```js
function sparkline(data, { width = 80, height = 20, color = '#666' } = {}) {
  const x = d3.scaleLinear().domain([0, data.length - 1]).range([1, width - 1]);
  const y = d3.scaleLinear().domain(d3.extent(data)).range([height - 1, 1]);
  const line = d3.line().x((_, i) => x(i)).y(d => y(d));

  return `<svg width="${width}" height="${height}" style="vertical-align: middle">
    <path d="${line(data)}" fill="none" stroke="${color}" stroke-width="1"/>
    <circle cx="${x(data.length - 1)}" cy="${y(data[data.length - 1])}" r="1.5" fill="${color}"/>
  </svg>`;
}
```

## Annotations

```js
svg.append('line')
  .attr('x1', xScale(annotX)).attr('x2', xScale(annotX))
  .attr('y1', yScale(annotY) - 15).attr('y2', yScale(annotY) - 3)
  .attr('stroke', TUFTE.axis).attr('stroke-width', 0.5);

svg.append('text')
  .attr('class', 'annotation')
  .attr('x', xScale(annotX))
  .attr('y', yScale(annotY) - 18)
  .attr('text-anchor', 'middle')
  .text('Peak: $4.2M');
```

## Responsive

```js
// Use viewBox for fluid scaling
svg.attr('viewBox', `0 0 ${width + margin.left + margin.right} ${height + margin.top + margin.bottom}`)
   .attr('width', '100%')
   .attr('height', null);
```

## Accessibility

```js
svg.attr('role', 'img')
   .attr('aria-label', 'Revenue grew 23% Q1-Q4, peaking at $4.2M in Q3');
```
