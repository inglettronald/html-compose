# Genre: postmortem (incident / root-cause writeup)

**When to use.** Something broke and you're explaining, after the fact, what
happened, why, and what changes so it doesn't recur. Blameless, factual,
timeline-driven.

**Stance.** Calm and precise. State impact plainly, separate symptom from cause,
and make the root cause unmissable. The reader should leave understanding the
*mechanism* of the failure, not just that it happened.

## Section skeleton

1. **Summary** — what broke, blast radius, duration, current status, in a few
   sentences. Lead with a `note` whose type reflects current state (`bad` if
   ongoing, `good` if resolved). A small `pillrow` of facts (severity, scope,
   duration) reads well here.
2. **Timeline** — a `flow` of events with times: detection → diagnosis →
   mitigation → resolution. Keep each step factual, one line of what happened.
3. **Root cause** — the mechanism, built up like an explainer: a `flow` or
   bespoke diagram showing how the conditions combined to produce the failure.
   End on the single `note bad` that names the actual cause.
4. **Contributing factors** — a severity `table` (`.sev` for each): what made it
   worse, slower to detect, or harder to fix. Separate from the root cause.
5. **What changes** — remediations as a `table` or `flow`, each with an owner /
   status pill. Distinguish "stops this exact bug" from "reduces the class."
6. **What went right** — short `note good`. Detection, tooling, or response that
   worked; worth preserving.

## Components used

`header` · section headings · `note` (bad for the cause, good for what worked,
warn for latent risk) · `flow` (timeline, root-cause mechanism) · severity
`table` + `legend` · `pillrow` (impact facts, remediation status) · bespoke
diagram · code block (for the failing snippet / log excerpt) · colophon.

## Notes specific to this genre

- Blameless: describe systems and decisions, never people. "The retry path
  lacked a cap," not "X forgot the cap."
- Symptom ≠ cause. The `note bad` in §3 is the *mechanism*, not the alert that
  fired. If §3 could be predicted from §2 plus the system's design, it's right.
- Use a code block with `.del`/`.add` to show the exact fix when there is one.
