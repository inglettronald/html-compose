# Genre: explainer (system map / mechanism)

**When to use.** The reader needs to *understand how something works* — a
subsystem, a pipeline, a mechanism, a data flow. No recommendation, no verdict.
The job is to leave the reader able to reason about the thing themselves.

**Stance.** Teacherly and structural. Build up from first principles or from the
entry point; make the shape of the system legible. Diagrams do heavy lifting.

## Section skeleton

Looser than `argument` — follow the system's own structure. A typical arc:

1. **The one idea** — the single mental model the rest hangs off. If the system
   changed recently, contrast old vs new here so the reader rebases.
2. **The pieces** — the components/fields/actors and what each is for. A
   `panel` + `table` is ideal: name on the left, role on the right.
3. **How it flows** — the runtime path, as a `flow`, paired with a tailored
   diagram when order/layering/state matters (search the library, e.g.
   `stack-layers`; invent one if nothing fits). This is the centre of an
   explainer; spend the most effort here.
4. **The sharp edges** — non-obvious gotchas, invariants, "you'd think X but
   actually Y" facts. `note info`/`note warn`. These are what readers come back
   for.
5. **Where to look** — the actual symbols/files, so the reader can go read code.
   A `table` or the colophon.

## Components used

`header` + pill TOC · section headings · `panel` + `table` (the pieces) ·
`flow` · `note` (info for pointers, warn for gotchas) · code block (lightweight
convention) · pills · colophon. Plus library visualizations (or fresh ones) —
diagrams do the heavy lifting in an explainer.

## Notes specific to this genre

- No `cards`, no recommendation, no `.sev` severity framing — nothing here is
  being judged as good/bad. Use `info`/`warn` only to mean "notable" / "watch
  out," not "this is wrong."
- Favour "you'd think X, but actually Y" `note info` blocks — surfacing the
  counter-intuitive is the whole value of an explainer.
- A diagram tailored to the topic beats a forced generic one. Search the
  library first; reach for `stack-layers` only when it genuinely fits, and
  invent (then offer to capture) when it doesn't.
