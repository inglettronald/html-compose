# mermaid-in-html

**communicates:** how to embed Mermaid in html-compose (or similar) HTML without
silent parse failures from the HTML layer or invalid classDiagram syntax.

**tags:** mermaid, classDiagram, flowchart, html, embedding, stereotypes,
text-plain, syntax

**fits:** argument, explainer, any v2 doc that swaps CSS UML for Mermaid.

**CSS:** none beyond existing `.mermaid-wrap` panel styling if desired.

## HTML pitfall — stereotypes

`<pre class="mermaid">` with `<<interface>>` breaks: the browser parses
`<interface>` as HTML. Mermaid then throws a syntax error with no obvious cause.

**Always** use a `text/plain` carrier for diagrams containing `<<…>>` or `<` in
labels, then inject a `.mermaid` div in JS before `mermaid.run()`.

## Markup pattern

```html
<div class="mermaid-wrap">
<script type="text/plain" class="mermaid-source">
classDiagram
  class Movable {
    <<interface>>
    +id()
  }
</script>
</div>

<script src="https://cdn.jsdelivr.net/npm/mermaid@10/dist/mermaid.min.js"></script>
<script>
document.querySelectorAll('script.mermaid-source').forEach(function (src) {
  var div = document.createElement('div');
  div.className = 'mermaid';
  div.textContent = src.textContent.trim();
  src.replaceWith(div);
});
mermaid.initialize({ startOnLoad: false, theme: 'base', /* themeVariables */ });
mermaid.run();
</script>
```

## classDiagram rules (learned the hard way)

| Avoid | Use instead |
|-------|-------------|
| `+draw(ctx, mx, my)` | `+draw()` |
| `+getScale() setScale()` on one line | two lines |
| `+renderHud placeholder if preview` | `+renderHudPreview()` |
| `+renderContainerInfo(...)` | `+renderContainerInfo()` |
| `Child ..\| Parent` | `Child ..\|> Parent` |

## Don't

- Don't rely on HTML entities (`&lt;&lt;interface&gt;&gt;`) inside `.mermaid` —
  Mermaid may receive literal entities, not `<<interface>>`.
- Don't use `startOnLoad: true` when injecting sources — convert scripts first,
  then `mermaid.run()` once.
