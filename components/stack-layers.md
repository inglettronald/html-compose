# stack-layers

**communicates:** what sits/draws on top of what — layer precedence, z-order,
before/after orderings, and "held aside / absent" state.

**tags:** layers, precedence, z-order, stack, before-after, rendering, hierarchy

**fits:** explainer (layering mechanisms), argument (before/after a change),
postmortem (state at point of failure).

**CSS:** base `style.css` (the `.stage`/`.stack`/`.layer`/`.axis` rules). No
extra CSS to inline beyond the base stylesheet.

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
