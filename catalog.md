# Base scaffolding components

The consistent furniture every document is built from — header, callouts,
tables, step flows, cards, pills, code. This is the **base layer**: it changes
only via deliberate tuning of `style.css`. All styling comes from `style.css`
(inlined into the output).

Tailored visualizations built for a specific communicative job (layer stacks,
before/after diagrams, savings charts, spatial layouts) are **not** here — they
live in the searchable component library at `components/`, each carrying its own
CSS in a `## CSS` section that is inlined alongside `style.css` only when used.
Search `components/INDEX.md` by intent, and invent a new one from tokens when
nothing fits (see SKILL.md).

The HTML skeleton every document starts from:

```html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Short descriptive title</title>
<style>
/* paste the full contents of style.css here */
</style>
</head>
<body>
<div class="wrap">
  <!-- header, then sections -->
</div>
</body>
</html>
```

---

## Document header + sticky TOC

Kicker (context line, mono) → title → one-sentence subtitle. The `nav.toc` is a
**sibling after** `<header>`, a direct child of `.wrap` — never inside the
header (sticky is confined to its parent's box, so a TOC inside the short header
would unstick the moment the header scrolls off). It pins to the top on scroll
and scrolls horizontally when entries overflow one line. Render entries as
compact divider-separated links (no pill chrome); keep each label short — a
number plus 2-4 words — so the bar rarely needs scrolling at all.

```html
<header class="head">
  <div class="kicker">Area · version / scope · branch or context</div>
  <h1>The thing this document is about</h1>
  <p class="sub">One sentence: what this maps/argues/reports and the angle it takes.</p>
</header>
<nav class="toc">
  <a href="#one">1 · First section</a>
  <a href="#two">2 · Second section</a>
</nav>
```

## Sticky TOC wheel script (progressive enhancement)

Optional but recommended whenever there's a `nav.toc`: lets a vertical mouse
wheel scroll the horizontal TOC while the pointer is over it. Pure inline, no
external deps; the document works fully without it. Place once before
`</body>`.

```html
<script>
(function(){
  var t=document.querySelector('nav.toc'); if(!t) return;
  t.addEventListener('wheel',function(e){
    if(!e.deltaY) return;
    var max=t.scrollWidth-t.clientWidth; if(max<=0) return;          // nothing to scroll
    var next=Math.max(0,Math.min(max,t.scrollLeft+e.deltaY));
    if(next!==t.scrollLeft){ t.scrollLeft=next; e.preventDefault(); } // let the page scroll at the ends
  },{passive:false});
})();
</script>
```

## Section heading (with anchor)

Every `h2`/`h3` gets an `id` and the leading anchor. `h2` carries a mono number.

```html
<h2 id="one"><a class="anchor" href="#one" aria-label="Permalink">#</a><span class="n">01</span>Section title</h2>
<p class="lede">One line framing what this section establishes.</p>

<h3 id="one-a"><a class="anchor" href="#one-a" aria-label="Permalink">#</a>Sub-heading</h3>
```

## Semantic callout

Pulls the eye to one fact. Tag + body. Type carries meaning — pick deliberately.

```html
<div class="note bad">      <!-- warn | bad | good | info -->
  <span class="tag">What this is</span>
  <p>The one fact, stated plainly.</p>
</div>
```

- `info` — a pointer / "you'll see this referenced" / notable detail
- `good` — why something is correct / the safe path / the win
- `warn` — fragile, caution, breaks silently on change
- `bad` — broken now, the thing that bites you

## Panel

Neutral container to group related content (often a table or tight diagram).
Add `.tight` for less padding.

```html
<div class="panel tight">
  <!-- table, list, or small diagram -->
</div>
```

## Table

Mono uppercase headers. Use a `.sev` cell for severity/status.

```html
<table>
  <tr><th style="width:20%">Column</th><th>What it means</th></tr>
  <tr>
    <td><code>thing</code></td>
    <td>Description. <span class="sev bad">● breaks now</span></td>
  </tr>
</table>
```

`.sev` variants: `bad` (breaks now), `warn` (fragile), `info` (coupling/watch),
`good` (safe).

## Dot legend

Explains the dots/severities used below it.

```html
<div class="legend">
  <span><span class="dot bad"></span>breaks now</span>
  <span><span class="dot warn"></span>fragile</span>
  <span><span class="dot info"></span>watch</span>
</div>
```

## Numbered flow

Ordered steps with a connector spine. For sequences, procedures, surgery steps.

```html
<ol class="flow">
  <li><b>Step name.</b> What happens in this step.</li>
  <li><b>Next step.</b> And so on.</li>
</ol>
```

## Comparison cards (pro / con / neu)

Side-by-side options. Mark the recommended one `.rec`; the other `.alt`.

```html
<div class="cards">
  <div class="card rec">
    <span class="badge">Option A · minimal</span>
    <h3 id="opt-a"><a class="anchor" href="#opt-a" aria-label="Permalink">#</a>Short name</h3>
    <p class="muted" style="font-size:13.5px">One-line summary of the approach.</p>
    <ul>
      <li class="pro">A concrete upside.</li>
      <li class="con">A concrete downside.</li>
      <li class="neu">A neutral tradeoff / consequence.</li>
    </ul>
  </div>
  <div class="card alt">
    <span class="badge">Option B · general</span>
    <h3 id="opt-b"><a class="anchor" href="#opt-b" aria-label="Permalink">#</a>Short name</h3>
    <p class="muted" style="font-size:13.5px">Summary.</p>
    <ul><li class="pro">…</li><li class="con">…</li></ul>
  </div>
</div>
```

## Pills

Compact tags. Add `accent-*` to attach one semantic meaning.

```html
<div class="pillrow">
  <span class="pill accent-info">DEFAULT → near the top</span>
  <span class="pill accent-good">BELOW → the bottom</span>
</div>
```

## Code block (lightweight convention)

Plain mono text. **Do not hand-colour keywords or strings.** Only four classes
carry meaning, used sparingly:

- `.c` — comment / annotation (faint)
- `.add` — added / correct line (green)
- `.del` — removed / wrong line (red)
- `.em` — the one thing to look at (blue)

```html
<pre><span class="c">// what this snippet shows</span>
void doThing(Placement p, Runnable render) {
    <span class="em">var snapshot = capture(state);</span>   <span class="c">// the load-bearing line</span>
    try { render.run(); }
    finally { restore(state, snapshot); }
}</pre>
```

Diff style:

```html
<pre><span class="del">- old line that was wrong</span>
<span class="add">+ new line that fixes it</span>
  unchanged context line</pre>
```

## Visualizations live in the library

For "what's on top of what" stacks, before/after diagrams, savings charts, and
other tailored pictures, search `components/INDEX.md` by intent (e.g.
`stack-layers` for layer precedence). If nothing fits, invent one from tokens.

## Colophon / sources footer

Closes a research-backed document. Mono, faint.

```html
<hr style="margin-top:60px">
<p class="colophon">Sources read: FileA · FileB · FileC. No code changed by this report.</p>
```
