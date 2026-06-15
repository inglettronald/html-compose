# html-compose

A Claude Code skill that authors **self-contained HTML documents** that read like
sharp engineering writeups: design/decision reports, system explainers,
postmortems, and release notes. One portable `.html` file, no external CSS, fonts,
or scripts.

It is built so the look and voice are *yours*. The aesthetic and editorial bar
live in two swappable files, and an onboarding flow sets them up by interview.
Everyone who installs it gets the same machinery and their own house style.

## Install

Copy this repo into your Claude Code skills directory so the folder name is the
skill name:

```bash
git clone https://github.com/<you>/html-compose.git ~/.claude/skills/html-compose
```

Or symlink a checkout you keep elsewhere:

```bash
ln -s "$(pwd)/html-compose" ~/.claude/skills/html-compose
```

Claude Code discovers it on next launch. Confirm with `/help` or by asking for an
HTML report.

## First run: make it yours

```
/html-compose onboard
```

This runs a short interview and writes your preference files: your voice into
`house-style.md`, and your theme, palette, and type into the `:root` tokens of
`style.css`. It also resets the component library to empty, since captured
components are personal. It never edits the skill's logic, so you can re-run it
any time and pull skill updates without losing your style.

You can skip onboarding and use the defaults immediately. Onboarding just changes
the starting point.

## Use it

Ask in plain language. The skill classifies the request into a genre and assembles
the document.

> Write up the caching redesign as an HTML decision report.

> Explain how the render pipeline works as a shareable page.

> Turn this incident into a postmortem.

Output is saved to `.claude/outputs/html/<title>.html` in the current project.

When you like a visualization it produced, say so ("save that", "add it to the
library") and it captures the component for reuse in later documents.

### Exercise the loop

```
/html-compose 5
```

Pass a bare number to generate that many example pages on fabricated topics,
spread across genres. It's a development mode for pressure-testing the base and
surfacing gaps in the component library. The pages land in
`.claude/outputs/html/example-NN-*.html`, and afterwards it asks which of the
components it invented you'd like to capture — nothing is added to the library
without your say-so.

## How it is organized

A small shared **base** plus two growing **libraries**:

| File / dir          | Role                                                        |
|---------------------|-------------------------------------------------------------|
| `SKILL.md`          | The orchestration: workflow, capture loop, governance       |
| `style.css`         | Visual identity (every tunable value is a `:root` token)    |
| `house-style.md`    | Editorial voice and the colour-discipline bar               |
| `catalog.md`        | Base scaffolding markup shared by every document            |
| `genres/`           | Whole-document structures, indexed in `genres/INDEX.md`     |
| `components/`        | Section-level visualizations, indexed in `components/INDEX.md` |

The base is consistent and tuned slowly. The libraries are extensible and
discovered by intent through their `INDEX.md`, so adding a genre or component
never means editing `SKILL.md`.

## Customize and extend

**Shift the whole look.** Edit a token in `style.css`. It propagates to every
document and every component that references it.

**Shift the voice.** Edit `house-style.md`.

**Add a genre.** Drop a `genres/<name>.md` (when to use, section skeleton,
components used) and add one line to `genres/INDEX.md`.

**Add a component.** Let the skill capture one for you, or write a
`components/<name>.md` and register it in `components/INDEX.md`. Components use
`var(--…)` tokens so your style tuning reaches them too.

## License

See [LICENSE](LICENSE).
