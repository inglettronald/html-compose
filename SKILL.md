---
name: html-compose
description: >
  Author a self-contained HTML document with a consistent
  base style and a growing, searchable library of tailored visual components —
  design/decision reports, system explainers, postmortems, and changelogs. Use
  when asked to produce an HTML report, writeup, explainer, design doc, system
  map, incident/root-cause writeup, or release notes; or to "visualize" a
  system, decision, or set of changes as a shareable web page. Not for slide
  decks, marketing pages, or app UI.
---

# html-compose

> **`/html-compose onboard`** → skip the workflow below and run [Onboarding](#onboarding-html-compose-onboard).

Produce one **self-contained HTML document** per the user's preferences. The skill
is a small shared **base** plus two growing **libraries**; keep them separate and
each stays healthy.

**Base** — shared by every document, tuned slowly:

- **Visual identity → [`style.css`](style.css).** Tokens for colour, type,
  spacing, layout, print. Read it for the look; it is not restated here.
- **Editorial voice → [`house-style.md`](house-style.md).** The prose and
  judgement bar. The swappable preference layer.
- **Scaffolding markup → [`catalog.md`](catalog.md).** The structural furniture
  (header, callouts, tables, flow, cards, code) every document is built from.

**Libraries** — extensible, grown over time, discovered *by intent* through an
INDEX (never enumerated here, so nothing drifts out of sync):

- **Genres → [`genres/INDEX.md`](genres/INDEX.md).** Whole-document structures,
  one per reader goal.
- **Components → [`components/INDEX.md`](components/INDEX.md).** Section-level
  tailored visualizations.

For either library: match an entry by reading its INDEX; invent or define a new
one when nothing fits; capture the good ones (below). There is no `examples/` —
nothing to imitate or keep in sync.

## Workflow

1. **Classify into a genre.** Read [`genres/INDEX.md`](genres/INDEX.md) and match
   the request to the reader's goal. If torn between two, ask. No good fit → pick
   the closest and adapt, or define a new genre (Governance).

2. **Read** [`house-style.md`](house-style.md) (the bar),
   [`catalog.md`](catalog.md) (scaffolding), `genres/<genre>.md` (structure),
   and [`components/INDEX.md`](components/INDEX.md) (what the library has). Read
   individual `components/<name>.md` only for the ones you'll use.

3. **Assemble section by section.** Structural furniture (header, callouts,
   tables, step flows) comes from `catalog.md`. For a *tailored visualization* —
   comparison, layout, before/after, saving, spatial picture:
   - **Search `components/INDEX.md` by intent** — the *thing being communicated*,
     not the topic. A match → use it, restyled through current tokens.
   - **No match → invent one** from tokens (`var(--…)`, never raw hex). This is
     the point of the library; a fresh one is a capture candidate (below).

4. **Inline everything.** Paste the full `style.css` into one `<style>` block,
   plus the CSS of any library component that carries its own. Add the sticky-TOC
   wheel script (`catalog.md`) before `</body>`. One portable file — no `<link>`,
   CDN, or external JS.

5. **Save** to `.claude/outputs/html/<kebab-title>.html` in the current project
   (create the dir if missing). Report the path.

## Capturing a component (phrase-triggered)

When the user signals they like a visualization — "save that", "add this to the
library" — capture it for reuse:

1. **Confirm scope.** Identify the exact markup region; propose a searchable
   **kebab name**, a one-line **`communicates:`** intent, and **tags**. Confirm.
2. **Write `components/<name>.md`** — the `communicates:` line, tags, markup, its
   CSS (inline if bespoke, or note "CSS: base style.css"), and which genres it
   suits. See [`components/stack-layers.md`](components/stack-layers.md) for the
   shape.
3. **Register it** in [`components/INDEX.md`](components/INDEX.md):
   `- <name> — <communicates> · tags: a, b, c`
4. **Tokens only.** A captured component uses `var(--…)`, never raw hex or fixed
   fonts, so base-style tuning propagates. Raw values live only in `style.css`.

Before adding something near an existing entry, extend that entry instead.

## Tuning the base (slow, deliberate)

Nudge the *whole look* ("wider columns", "shift this blue", "more padding") by
editing the relevant token or rule in [`style.css`](style.css) — it propagates to
every document and every token-built component, including old ones. Keep these
edits intentional and rare. To shift the *voice* instead, edit
[`house-style.md`](house-style.md).

## Hard rules (every output)

- **Self-contained.** Inline all CSS and JS. No `<link>`, remote fonts, or CDN
  scripts. A small **inline** script is allowed only as progressive enhancement —
  the document must work fully without it (e.g. the TOC wheel handler).
- **Screen and print both come from `style.css`.** It ships an `@media print`
  block alongside the screen tokens — don't fight it or hand-style for print.
- **Heading anchors.** Every `h2`/`h3` gets an `id` and a leading
  `<a class="anchor" href="#id" aria-label="Permalink">#</a>`.
- **Code blocks follow the `catalog.md` convention.** Plain mono text; only
  `.c`/`.add`/`.del`/`.em` carry meaning. No hand-coloured keywords or strings.
- **Meet the editorial bar** in [`house-style.md`](house-style.md), including its
  colour discipline: before finishing, audit that every coloured element maps to
  exactly one of warn/good/bad/info, and recolour or de-colour the rest.

## Governance (keep the layers healthy)

- **Base stays small.** New colours/fonts/radii/spacing go in `style.css` tokens
  or they don't exist — never in a genre or component file.
- **Library entries must be searchable.** No `communicates:` line and tags means
  unfindable, so it's not done. Name by communicative job, not first topic.
- **Adding a genre:** drop `genres/<name>.md` (when to use · section skeleton ·
  base components used · genre-specific structure) and register it in
  `genres/INDEX.md` — the same capture loop as a component.

## Onboarding (`/html-compose onboard`)

Personalize the skill for a new user. This writes **only** the two preference
surfaces — [`house-style.md`](house-style.md) (voice) and the `:root` tokens in
[`style.css`](style.css) (look). Never touch `SKILL.md`, `catalog.md`, or
`genres/` — those are mechanism, shared by everyone.

Run it as a short interview, then apply the answers:

1. **Frame it.** One line: "I'll set up your voice and visual identity. Two
   files change; the skill's logic doesn't." Then ask the questions below with
   `AskUserQuestion`, batched, with the current defaults pre-selected.

2. **Voice & audience** → rewrites `house-style.md`.
   - Who reads these, and in what register? (peer engineers / mixed eng+product
     / leadership / external) — sets the tone line.
   - Density: terse-and-dense vs. more explanatory.
   - Anything to forbid or require (emoji, first person, hedging, jargon).

3. **Look** → edits `style.css` `:root` tokens (and the `@media print` block to
   stay coherent).
   - Base theme: dark or light surfaces.
   - Accent / semantic palette: keep the warn/good/bad/info defaults or supply
     four hexes (the meanings stay fixed — only the hues change).
   - Type: default system sans+mono stacks or custom families.
   - Frame: default width/rhythm or tighter/looser.

4. **Apply.** Rewrite `house-style.md`'s bullets in their words (keep the
   structure: density, lede, prefer-component, measure, concrete, voice,
   one-callout, earn-the-colour). Edit only the relevant `style.css` tokens —
   leave layout/scaffolding rules alone. Keep semantic colour meaning-bound
   regardless of hue.

5. **Preview.** Offer to generate a one-page sample document (any genre) so they
   can see their identity before committing. Save under
   `.claude/outputs/html/onboarding-preview.html`.

Onboarding sets defaults; everything stays editable later via *Tuning the base*
and by hand-editing `house-style.md`.
