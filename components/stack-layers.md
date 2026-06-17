# stack-layers

**communicates:** what sits/draws on top of what — layer precedence, z-order,
before/after orderings, and "held aside / absent" state.

**tags:** layers, precedence, z-order, stack, before-after, rendering, hierarchy

**fits:** explainer (layering mechanisms), argument (before/after a change),
postmortem (state at point of failure).

**CSS:** carried by this component (see [CSS](#css) below) — inline it alongside
`style.css` whenever this component is used. References base tokens only for the
semantic palette; the surface shades are local.

## What it is

A labelled vertical stack of layers, optionally paired with a direction axis and
multiple side-by-side columns to show a transition (before → after). Use
`accent-*` to give a layer one semantic meaning; `.raised` for a lifted surface;
`.ghost` for a held-aside / absent layer. Markup order is top-to-bottom on
screen — reverse your list if you want bottom-first source order.

## Markup

Single stack with an axis:

```html
<div class="stage panel">
  <div class="axis"><span class="top">▲ on top</span><span>▼ back</span></div>
  <div class="stack-col">
    <h4>Caption</h4>
    <div class="stack">
      <div class="layer accent-info"><span><b>Top layer</b></span><small>note</small></div>
      <div class="layer raised"><span><b>Middle</b></span><small>note</small></div>
      <div class="layer ghost"><span>absent / held aside</span><small>snapshot</small></div>
    </div>
  </div>
</div>
```

Before → after, two columns side by side:

```html
<div class="stage">
  <div class="stack-col">
    <h4>Before</h4>
    <div class="stack">
      <div class="layer raised"><span><b>A</b></span><small></small></div>
      <div class="layer raised"><span><b>B</b></span><small></small></div>
    </div>
  </div>
  <div class="stack-col">
    <h4>After</h4>
    <div class="stack">
      <div class="layer raised"><span><b>A</b></span><small></small></div>
      <div class="layer accent-good"><span><b>new layer</b></span><small>inserted</small></div>
      <div class="layer raised"><span><b>B</b></span><small></small></div>
    </div>
  </div>
</div>
```

## Don't

- Don't use topic names as classes (`mc`, `lunar`, …). Use `accent-*` for
  meaning so the component stays reusable.
- Don't reach for this when the relationship isn't a linear stack — invent a
  bespoke diagram from tokens instead.

## CSS

Inline this block alongside `style.css` when the component is used. It carries
its own responsive and print overrides.

```css
/* stack-layers — vertical layer/precedence stacks.
   accent-* = one semantic meaning; .raised = lifted surface;
   .ghost = held-aside/absent. Semantic colour via base tokens. */
.stage{display:flex;gap:26px;flex-wrap:wrap;align-items:stretch}
.stack-col{flex:1;min-width:var(--col-min)}
.stack-col h4{font-family:var(--mono);font-size:var(--fs-mono-sm);letter-spacing:.1em;text-transform:uppercase;color:var(--ink-dim);margin:0 0 10px;font-weight:500}
.stack{display:flex;flex-direction:column;border:1px solid var(--line);border-radius:9px;overflow:hidden;background:var(--panel-2)}
.layer{padding:9px 13px;border-top:1px solid var(--line);font-size:var(--fs-pre);display:flex;justify-content:space-between;align-items:center;gap:8px}
.layer:first-child{border-top:none}
.layer b{font-weight:600}
.layer small{color:var(--ink-faint);font-family:var(--mono);font-size:var(--fs-label)}
.layer.raised{background:#141417}
.layer.accent-info{box-shadow:inset 2px 0 0 var(--info)}
.layer.accent-good{box-shadow:inset 2px 0 0 var(--good)}
.layer.accent-warn{box-shadow:inset 2px 0 0 var(--warn);color:var(--ink-dim)}
.layer.accent-bad{box-shadow:inset 2px 0 0 var(--bad)}
.layer.ghost{background:repeating-linear-gradient(135deg,#0a0a0b,#0a0a0b 6px,#0e0e10 6px,#0e0e10 12px);color:var(--ink-faint)}
.axis{display:flex;flex-direction:column;justify-content:space-between;font-family:var(--mono);font-size:10px;color:var(--ink-faint);text-align:right;padding:2px 0;white-space:nowrap}
.axis .top{color:var(--ink-dim)}
@media(max-width:760px){.stage{flex-direction:column}}
@media print{
  .stack,.stage{break-inside:avoid;page-break-inside:avoid}
  .layer.ghost{background:repeating-linear-gradient(135deg,#eee,#eee 6px,#f6f6f7 6px,#f6f6f7 12px)}
}
```
