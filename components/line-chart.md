# line-chart

**communicates:** one or more series plotted over a shared axis — comparing
trajectories, trends, rates, or crossover between quantities. A hand-authored,
static inline SVG line chart.

**tags:** chart, line-chart, svg, series, trend, time-series, comparison,
trajectory, plot, curves, rate, decay, growth, crossover, threshold

**fits:** explainer (how a quantity moves over time/steps), argument
(before/after a change, projected vs actual), postmortem (a metric's path around
an incident).

**CSS:** carried by this component (see [CSS](#css) below) — inline it alongside
`style.css` whenever this component is used. Series colours and reference marks
reference base tokens, so the chart re-themes with the document.

## What it is

A line graph drawn as inline SVG — no plotting library, no script. Each series
is one `<polyline>` whose `points` you precompute. Optional parts, add only what
the story needs: a dashed `.ref` line for a threshold/target, `.mk` drop-lines
for a notable x (crossover, half-life, an event), `circle` dots for keypoints,
and axis ticks. Use it when the story is the *shape / relationship between
series*, not exact values (those belong in a table).

Series colours encode identity, a legitimate data-viz use of the palette:
default to one accent (`.s1` = `--info`, the lead series) plus a neutral
(`.s2` = `--ink-dim`). Add `.s3`/`.s4` (good/warn) only when a third/fourth
series genuinely needs distinguishing. Pair with a `.legend` naming each series.
One series is fine (a single trend); the component is not limited to comparison.

## Computing the points

The chart is a fixed pixel box; map data to it once and paste the `points`.
With plot area `x∈[X0,X1]`, `y∈[Y0,Y1]`, and value `v` normalised to `[0,1]`
(`v = (raw − min)/(max − min)`):

```
x(i) = X0 + i * (X1 - X0) / N        // N = number of steps; or map a real x the same way
y(v) = Y1 - v * (Y1 - Y0)            // Y0 = top (v=1), Y1 = baseline (v=0)
```

Two common shapes:
- **Linear / arbitrary data** — normalise each value with the formula above.
- **Geometric decay/growth** `v(i) = r^i` — half-life (or doubling) in steps is
  `ln(0.5)/ln(r)` (e.g. r=0.91 → ~7.3 steps; r=0.546 → ~1.15). Drop a `.mk` line
  at that x to mark it.

## Markup

```html
<div class="linechart">
  <svg viewBox="0 0 660 248" role="img"
       aria-label="Series A vs series B over 14 steps">
    <!-- axes -->
    <line class="grid" x1="44" y1="20" x2="44" y2="220"/>
    <line class="grid" x1="44" y1="220" x2="640" y2="220"/>
    <!-- optional reference threshold/target -->
    <line class="ref" x1="44" y1="120" x2="640" y2="120"/>
    <text x="48" y="116">target</text>
    <!-- optional markers at a notable x (crossover, event) -->
    <line class="mk" x1="346.4" y1="20" x2="346.4" y2="220"/>
    <!-- draw lower/baseline series first (sits under) -->
    <polyline class="s2" points="44,20 85.1,110.8 126.3,160.4 167.4,187.4 208.6,202.2 249.7,210.3 290.9,214.7 332,217.1 373.1,218.4 414.3,219.1 455.4,219.5 496.6,219.7 537.7,219.8 578.9,219.9 620,220"/>
    <!-- lead series on top -->
    <polyline class="s1" points="44,20 85.1,38 126.3,54.4 167.4,69.3 208.6,82.8 249.7,95.1 290.9,106.4 332,116.6 373.1,125.9 414.3,134.4 455.4,142.1 496.6,149.1 537.7,155.5 578.9,161.3 620,166.6"/>
    <circle class="dot1" cx="346.4" cy="116.6" r="3"/>
    <!-- inline series captions + axis ticks -->
    <text class="lbl s1-fill" x="430" y="150">series A</text>
    <text class="lbl" x="150" y="205">series B</text>
    <text x="40" y="24" text-anchor="end">max</text>
    <text x="40" y="223" text-anchor="end">0</text>
    <text x="44" y="236">0</text>
    <text x="332" y="236" text-anchor="middle">7</text>
    <text x="620" y="236" text-anchor="middle">14</text>
  </svg>
  <div class="legend" style="justify-content:center;margin-top:6px">
    <span><span class="dot info"></span>series A</span>
    <span><span class="dot" style="background:var(--ink-dim)"></span>series B</span>
  </div>
</div>
```

## Don't

- Don't use it for precise-value reading — it shows shape/relationship. Exact
  numbers belong in a table or a stat.
- Don't colour series with topic-named classes; keep `.s1`–`.s4` so the palette
  stays semantic and re-themeable.
- Don't reach for an animated/scripted chart — this is static SVG by design
  (self-contained, print-safe). Interactivity is a different component.
- Don't overload one chart with many series; past ~4 lines it stops reading.

## CSS

Inline this block alongside `style.css` when the component is used. It carries
its own responsive and print overrides.

```css
/* line-chart — static inline SVG line graph for 1+ series over a shared axis.
   Series colour = identity (.s1 info, .s2 neutral, .s3 good, .s4 warn).
   All colour via base tokens, so it re-themes (incl. the print stylesheet). */
.linechart{background:var(--panel);border:1px solid var(--line);border-radius:var(--radius);padding:18px 20px 14px;margin:var(--space-block) 0}
.linechart svg{display:block;width:100%;height:auto;max-width:680px;margin:0 auto}
.linechart .grid{stroke:var(--line);stroke-width:1}
.linechart .ref{stroke:var(--line-2);stroke-width:1;stroke-dasharray:3 3}
.linechart .mk{stroke:var(--ink-faint);stroke-width:1;stroke-dasharray:2 3}
.linechart .s1{fill:none;stroke:var(--info);stroke-width:2}
.linechart .s2{fill:none;stroke:var(--ink-dim);stroke-width:2;stroke-dasharray:5 3}
.linechart .s3{fill:none;stroke:var(--good);stroke-width:2}
.linechart .s4{fill:none;stroke:var(--warn);stroke-width:2}
.linechart .dot1{fill:var(--info)} .linechart .dot2{fill:var(--ink-dim)}
.linechart text{font-family:var(--mono);font-size:10px;fill:var(--ink-faint)}
.linechart text.lbl{fill:var(--ink-dim)}
.linechart text.s1-fill{fill:var(--info)}
@media print{
  .linechart{break-inside:avoid;page-break-inside:avoid}
}
```
