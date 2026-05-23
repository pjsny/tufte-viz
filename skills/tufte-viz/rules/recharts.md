# Recharts: Tufte Configuration

Load this file when generating any Recharts (React) chart code.

## Tufte constants

```tsx
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
```

## Component configuration

### CartesianGrid

Always remove or make invisible:

```tsx
<CartesianGrid stroke="none" />
```

If gridlines are truly needed for reading precision:

```tsx
<CartesianGrid
  vertical={false}
  stroke={TUFTE.grid}
  strokeOpacity={0.1}
  strokeWidth={0.5}
/>
```

### XAxis

```tsx
<XAxis
  dataKey="name"
  tickLine={false}
  axisLine={{ stroke: TUFTE.axis, strokeWidth: 0.5 }}
  tick={{ fontSize: 12, fill: TUFTE.textTertiary, fontFamily: TUFTE.fontSans }}
  dy={8}
/>
```

### YAxis

```tsx
<YAxis
  tickLine={false}
  axisLine={false}
  tick={{ fontSize: 12, fill: TUFTE.textTertiary, fontFamily: TUFTE.fontSans }}
  width={45}
  dx={-4}
/>
```

### Tooltip (custom minimal)

```tsx
const TufteTooltip = ({ active, payload, label }: any) => {
  if (!active || !payload?.length) return null;
  return (
    <div style={{
      fontFamily: TUFTE.font,
      fontSize: 13,
      color: TUFTE.text,
      background: 'none',
      border: 'none',
      boxShadow: 'none',
      padding: 0,
    }}>
      <span style={{ color: TUFTE.textSecondary }}>{label}</span>{' '}
      <strong>{payload[0].value}</strong>
    </div>
  );
};
```

### Legend

Remove entirely. Use `<LabelList>` or custom `<text>` for direct labels:

```tsx
<Line dataKey="revenue" stroke={TUFTE.highlight} strokeWidth={1.5} dot={false}>
  <LabelList
    dataKey="revenue"
    position="right"
    content={({ x, y, value, index }) =>
      index === data.length - 1 ? (
        <text x={x + 8} y={y} fill={TUFTE.text} fontSize={12} fontFamily={TUFTE.font}>
          Revenue: {value}
        </text>
      ) : null
    }
  />
</Line>
```

### Line

```tsx
<Line
  dataKey="value"
  stroke={TUFTE.seriesDefault}
  strokeWidth={1.5}
  dot={false}
  activeDot={{ r: 3, fill: TUFTE.text, stroke: 'none' }}
/>
```

### Bar

```tsx
<Bar
  dataKey="value"
  fill={TUFTE.seriesDefault}
  radius={[1, 1, 0, 0]}
>
  <LabelList dataKey="value" position="top" fontSize={11} fill={TUFTE.textSecondary} />
</Bar>
```

## Small multiples layout

```tsx
<div style={{
  display: 'grid',
  gridTemplateColumns: 'repeat(2, 1fr)',
  gap: '8px 16px',
  background: TUFTE.bg,
  padding: 16,
}}>
  {categories.map((cat, i) => (
    <div key={cat}>
      <div style={{
        fontFamily: TUFTE.font,
        fontSize: 14, color: TUFTE.text, marginBottom: 4,
      }}>
        {cat}
      </div>
      <ResponsiveContainer width="100%" height={150}>
        <LineChart data={data[cat]}>
          <CartesianGrid stroke="none" />
          <XAxis
            dataKey="x"
            tickLine={false}
            axisLine={i >= categories.length - 2 ? { stroke: TUFTE.axis, strokeWidth: 0.5 } : false}
            tick={i >= categories.length - 2 ? { fontSize: 10, fill: TUFTE.textTertiary } : false}
          />
          <YAxis
            domain={sharedDomain}
            tickLine={false}
            axisLine={false}
            tick={i % 2 === 0 ? { fontSize: 10, fill: TUFTE.textTertiary } : false}
            width={i % 2 === 0 ? 35 : 5}
          />
          <Line dataKey="value" stroke={TUFTE.seriesDefault} strokeWidth={1.5} dot={false} />
        </LineChart>
      </ResponsiveContainer>
    </div>
  ))}
</div>
```
