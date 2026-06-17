# uml-class-diagram

**communicates:** class and interface relationships — architecture shape, adapter
seams, implementation tiers, and contrasting runtime paths (preview vs live,
before vs after refactor).

**tags:** uml, class-diagram, architecture, interfaces, adapters, refactor-shape,
api-sketch, dependency, seam

**fits:** argument (target shape for a PR), explainer (how types relate),
refactor plans where a code block would be too long and imprecise.

**CSS:** carried by this component (see [CSS](#css) below) — inline it alongside
`style.css` whenever this component is used. References base tokens only for the
semantic palette; the tint borders are local.

## What it is

A tiered UML-style class diagram built from HTML — no SVG or image export
required. Stack `.uml-tier` rows top-to-bottom; separate tiers with
`.uml-connector` labels (`uses`, `implements`, `delegates to`). Class boxes use
mono type notation (`+` public, `−` private). Optional `.uml-row2` at the bottom
contrasts two pipelines side-by-side (e.g. preview path vs live path).

### Class box variants

| Class | Meaning |
|-------|---------|
| `.uml-class.if` | Interface or contract (`«interface»` in `.st`) |
| `.uml-class.impl` | Concrete class (default solid border) |
| `.uml-class.legacy` | Backing store / deprecated seam (`«anchor»` or `«legacy»` in `.st.warn`) |

Use `.uml-line.vis` for comment lines (`// …`). Split fields from methods with
`.uml-sep`. Close with `.uml-key` when multiple variants appear in one diagram.

## Markup — minimal two-tier diagram

```html
<div class="uml">
  <div class="uml-caption">Optional caption · mono uppercase</div>
  <div class="uml-canvas">

    <div class="uml-tier">
      <div class="uml-class impl">
        <div class="uml-head">ClientShell</div>
        <div class="uml-body">
          <div class="uml-line">− editor: EditorEngine</div>
          <div class="uml-sep"></div>
          <div class="uml-line">+ draw(ctx)</div>
        </div>
      </div>
      <div class="uml-class impl">
        <div class="uml-head">EditorEngine</div>
        <div class="uml-body">
          <div class="uml-line">+ drawOverlay(ctx)</div>
          <div class="uml-line vis">// domain-agnostic input logic</div>
        </div>
      </div>
    </div>

    <div class="uml-connector">uses <b>Collection&lt;Item&gt;</b> from ▼</div>

    <div class="uml-tier">
      <div class="uml-class if">
        <div class="uml-head"><span class="st">«interface»</span> Item</div>
        <div class="uml-body">
          <div class="uml-line">+ id(): Object</div>
          <div class="uml-line">+ bounds(): Rect</div>
        </div>
      </div>
      <div class="uml-class impl">
        <div class="uml-head">ItemAdapter</div>
        <div class="uml-body">
          <div class="uml-line">− backing: LegacyType</div>
          <div class="uml-sep"></div>
          <div class="uml-line">implements Item</div>
        </div>
      </div>
      <div class="uml-class legacy">
        <div class="uml-head"><span class="st warn">«legacy»</span> LegacyType</div>
        <div class="uml-body">
          <div class="uml-line">persist + render today</div>
          <div class="uml-line vis">keep until migration done</div>
        </div>
      </div>
    </div>

    <div class="uml-key">
      <span><span class="sw if"></span> interface</span>
      <span><span class="sw legacy"></span> legacy / anchor</span>
    </div>
  </div>
</div>
```

## Markup — optional path comparison row

Add below the last tier when two runtime paths diverge (preview vs live, old vs
new call chain):

```html
    <div class="uml-row2">
      <div class="uml-path preview">
        <h5>Editor / preview path</h5>
        <ol>
          <li>Step one</li>
          <li>Step two</li>
        </ol>
      </div>
      <div class="uml-connector" style="align-self:center;padding:0 4px">≠</div>
      <div class="uml-path live">
        <h5>Live runtime path</h5>
        <ol>
          <li>Step one</li>
          <li>Step two</li>
        </ol>
      </div>
    </div>
```

`.uml-path.preview` tints with info; `.uml-path.live` tints with good — use
that pairing when one path is the target state.

## Don't

- Don't put domain-specific class names in CSS — only in markup content.
- Don't use this for sequence/timeline stories — use `flow` or invent a sequence
  component. This is for **type shape**, not step order over time.
- Don't exceed ~6 boxes per tier without splitting into two diagrams — readability
  drops fast on narrow viewports (tiers wrap, which is fine; dense grids are not).

## CSS

Inline this block alongside `style.css` when the component is used. It carries
its own responsive and print overrides.

```css
/* uml-class-diagram — tiered class/interface boxes, connector labels,
   optional path comparison. .if = interface · .impl = concrete ·
   .legacy = anchor/backing store. Semantic colour via base tokens. */
.uml{margin:var(--space-block) 0;font-family:var(--mono);font-size:11.5px;line-height:1.45}
.uml-caption{font-family:var(--mono);font-size:var(--fs-mono-sm);letter-spacing:.08em;text-transform:uppercase;color:var(--ink-dim);margin:0 0 12px}
.uml-canvas{background:var(--panel);border:1px solid var(--line);border-radius:var(--radius);padding:24px 20px 20px;overflow-x:auto}
.uml-tier{display:flex;flex-wrap:wrap;gap:14px;justify-content:center;align-items:flex-start;margin-bottom:8px}
.uml-tier-label{width:100%;text-align:center;font-size:10px;letter-spacing:.12em;text-transform:uppercase;color:var(--ink-faint);margin:4px 0 10px}
.uml-class{flex:0 1 220px;min-width:180px;max-width:260px;border:1px solid var(--line-2);border-radius:7px;background:var(--panel-2);color:var(--ink-dim)}
.uml-class.if{border-color:#1a3a5c;box-shadow:inset 0 2px 0 var(--info)}
.uml-class.impl{border-style:solid}
.uml-class.legacy{border-style:dashed;border-color:var(--warn);opacity:.92}
.uml-head{padding:7px 10px;border-bottom:1px solid var(--line);background:var(--panel);color:var(--ink);font-weight:600;font-size:12px}
.uml-head .st{font-weight:400;color:var(--info);font-size:10px}
.uml-head .st.warn{color:var(--warn)}
.uml-body{padding:6px 10px 8px}
.uml-sep{border-top:1px solid var(--line);margin:5px 0;padding-top:5px}
.uml-line{padding:1px 0;word-break:break-word}
.uml-line.vis{color:var(--ink-faint)}
.uml-connector{text-align:center;color:var(--ink-faint);font-size:10px;letter-spacing:.06em;padding:6px 0;line-height:1.3}
.uml-connector b{color:var(--ink-dim);font-weight:500}
.uml-row2{display:grid;grid-template-columns:1fr auto 1fr;gap:12px;align-items:start;margin-top:18px;padding-top:18px;border-top:1px dashed var(--line)}
.uml-path{padding:12px 14px;border:1px solid var(--line);border-radius:8px;background:var(--panel-2)}
.uml-path h5{margin:0 0 8px;font-size:11px;letter-spacing:.1em;text-transform:uppercase;color:var(--ink-dim);font-weight:500}
.uml-path ol{margin:0;padding-left:16px;color:var(--ink-dim)}
.uml-path li{margin:4px 0}
.uml-path.live{border-color:#1f3a24}
.uml-path.live h5{color:var(--good)}
.uml-path.preview{border-color:#16324f}
.uml-path.preview h5{color:var(--info)}
.uml-key{display:flex;flex-wrap:wrap;gap:14px;margin-top:14px;font-size:10px;color:var(--ink-faint)}
.uml-key span{display:flex;align-items:center;gap:6px}
.uml-key .sw{width:22px;height:10px;border-radius:2px;border:1px solid var(--line-2)}
.uml-key .sw.if{border-color:var(--info);box-shadow:inset 0 2px 0 var(--info)}
.uml-key .sw.legacy{border-style:dashed;border-color:var(--warn)}
@media(max-width:760px){.uml-row2{grid-template-columns:1fr}}
@media print{.uml-canvas{break-inside:avoid;page-break-inside:avoid}}
```
