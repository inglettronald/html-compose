# House style — the editorial bar

The **voice and judgement** layer for html-compose documents, and the one meant to be
swapped: rewrite this file to make every document read in your house's voice
without touching `SKILL.md` or the orchestration logic. The visual identity
(colour, type, spacing) lives in `style.css`; this file governs prose.

There is deliberately no exemplar HTML — this bar replaces a sample. Match it.

- **Density: high signal, no filler.** Every paragraph earns its place. No
  throat-clearing intros, no "in this section we will," no restating the title.
- **Lead each section with a `.lede`** — one line framing what the section
  establishes, then get into it.
- **Prefer a component over a paragraph when it carries the point better.** A
  table, callout, flow, or visualization beats three sentences describing the
  same structure. Prose connects and argues; components show.
- **Prose stays at `--measure` width** (~80ch). Don't write wall-to-wall.
- **Concrete over abstract.** Name the real symbol, file, field, number. "It
  smells" is not a finding; "no `try/finally`, so a throw leaves state X" is.
- **Voice: a sharp senior engineer explaining to a peer.** Direct, opinionated
  where the genre allows it, never breezy or marketing-toned. No emoji.
- **One strong callout per crux**, not a wall of them — overuse flattens signal.
- **Earn the colour.** Each semantic colour (`--warn`/`--good`/`--bad`/`--info`)
  means exactly one thing; if an element's colour carries no meaning, it's ink.
