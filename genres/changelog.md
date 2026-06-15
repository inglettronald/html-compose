# Genre: changelog (release notes)

**When to use.** Communicating what changed in a release or over a span of work,
grouped for fast scanning. Visually the lightest genre — mostly grouped tables
and pills, little prose.

**Stance.** Terse and scannable. The reader is hunting for "does this affect
me?" Lead with breaking changes; make each entry a single self-contained line.

## Section skeleton

1. **Header** — version, date, scope in the kicker; one-line summary as `.sub`.
   If there are breaking changes, a `note bad`/`warn` right under the header
   pointing to the breaking section.
2. **Highlights** (optional) — 2-4 headline changes as a short `flow` or pill
   row, for readers who won't scroll.
3. **Grouped changes** — one `h3` + `table` per category, in priority order:
   - **Breaking** — `note warn` framing; what changed and the migration.
   - **Added** — new features/APIs.
   - **Changed** — behaviour changes that aren't breaking.
   - **Fixed** — bug fixes.
   - **Internal / chore** — refactors, deps, tooling (last, often collapsible
     in spirit — keep it brief).
   Omit empty categories.
4. **Upgrade notes** (optional) — a `flow` of steps if upgrading needs action.

## Components used

`header` · `note` (warn/bad for breaking) · `h3` per category · `table`
(the change list) · `pillrow` (tags: area, PR/issue refs) · `flow` (upgrade
steps) · colophon (date / compare range).

## Change-table shape

Keep it to a few columns so it stays scannable:

```html
<h3 id="added"><a class="anchor" href="#added" aria-label="Permalink">#</a>Added</h3>
<table>
  <tr><th style="width:22%">Change</th><th style="width:14%">Area</th><th>Detail</th></tr>
  <tr>
    <td>Short title</td>
    <td><span class="pill">rendering</span></td>
    <td>One line on what it does and who it affects. <code>SymbolRef</code> if useful.</td>
  </tr>
</table>
```

## Notes specific to this genre

- Severity colour is reserved for *breaking* changes (warn/bad). Added/Changed/
  Fixed entries are neutral ink — don't paint a normal feature green.
- One line per entry. If an entry needs a paragraph, it wants its own explainer
  or argument doc; link to it instead.
- Order categories by reader urgency (breaking first), not by count.
