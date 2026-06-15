# Genre: argument (design / decision report)

**When to use.** You are recommending a change, or choosing between options, and
the reader needs to *follow the reasoning*, not just hear the conclusion. Maps a
system, exposes what's wrong or risky, states the constraints, lays out options,
and recommends one with a sequence to execute it. This is the flagship genre for
the skill's voice — lean hardest on the editorial bar (`house-style.md`) here.

**Stance.** Opinionated but honest. Show the failure modes with severity, give
the rejected option a fair hearing, then commit to a recommendation and say why.

## Section skeleton

The ordered H2s. Adapt names to the topic; keep the arc.

1. **The ground truth** — the system / constraint everything else follows from.
   Start from the current reality, especially if it changed recently. End the
   section with the one fact that breaks the obvious approach (a `note bad`).
2. **Where the change cuts in** — the specific site(s) and why the change has to
   happen there. A table of sites with status pills if there are several.
3. **What it does today, step by step** — a `flow` of the current mechanism,
   paired with a before/after visualization if depth/order matters (search the
   library, e.g. `stack-layers`; invent one if nothing fits).
4. **Why it smells** — concrete failure modes, not "it's ugly." A `legend` + a
   table where every row has a `.sev` and *what actually goes wrong*.
5. **Constraints any solution must respect** — a `panel` list. This is the box
   the options have to fit in; it makes the recommendation feel forced, not
   arbitrary.
6. **The options** — `cards` (rec + alt). Then, if a tempting wrong shape
   exists, a short table contrasting it with the chosen one. End with a concrete
   sketch (code block, lightweight convention) of the recommended option.
7. **Recommendation & sequencing** — a `note good` with the call, a paragraph
   tying it back to the constraints, and a `flow` of the PR/rollout order.

Close with the sources colophon if the report is research-backed.

## Components used

`header` + sticky TOC · section headings · `note` (bad to mark the breaking
fact, warn for fragility, good for the recommendation) · `flow` · severity
`table` + `legend` · comparison `cards` · contrast `table` · `pill` status row ·
code block · colophon. Plus a library visualization (or a fresh one) for any
before/after or structural picture.

## Notes specific to this genre

- The recommendation must be *derivable* from the constraints section. If a
  reader who only read §5 could predict §7, the argument is tight.
- Prefer one strong `note bad` per "this is the crux" moment. Overusing it
  flattens the signal.
- The code sketch is illustrative — label it as such ("names illustrative, not
  final") so it isn't read as a finished API.
