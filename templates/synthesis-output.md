# Synthesis Output Template

Copy this into `workspace/research/{slug}/playbook.md` and fill it in.
Delete bracketed guidance as you go. Keep the section order — it's the
order a reader needs: bottom line first, uncertainty never buried.

```markdown
# {Topic} — Tiger Research Playbook

> Research date: {YYYY-MM-DD} · {N} tigers ({list archetypes}) · scope: {one line}

## 1. TL;DR
{3-5 lines. The bottom line and the single most important action. If a
tiger found a security / time-sensitive / high-dollar / fact-conflict
issue, it goes HERE, not buried below.}

## 2. Your profile
{1 paragraph from the setup/context tiger. The specific, cited facts
about the user's situation that weight everything below. No generic
"you're a busy founder" filler — real constraints only.}

## 3. Cross-validated (high confidence)
{3-7 findings 2+ tigers independently agreed on. Lead with these. Tag
each with which tigers concurred.}

| Finding | Tigers |
|---|---|
| ... | A, C, D |

## 4. What each tiger added (deltas)
| Tiger | Unique contribution beyond the others |
|---|---|
| 🐯 A {name} | ... |
| 🐯 B {name} | ... |

## 5. Contested
{Where tigers disagreed. State the disagreement plainly. Pick a side
only if you can defend it with cited evidence; otherwise present both
and say what would resolve it.}

## 6. Concrete recommendations (prioritized)
1. {Action} — {why} — {tiger source} — {reversibility / risk}
2. ...

## 7. Open questions / professional follow-up
{What the tigers could not verify. Where the user needs a licensed
expert, domain authority, or data the tigers couldn't access. Be
explicit; do not paper over gaps.}

## 8. Tiger source attribution
- 🐯 A {name} — {what it researched, what it found of value}
- 🐯 B {name} — {...}
```

## Length targets

| Tigers | Playbook length |
|---|---|
| 2 | 800-1200 words |
| 3 | 1200-1800 words |
| 4-5 | 1500-2500 words |
| 6 | 2000-3000 words |

Shorter than the range → you're under-using the tigers. Longer → you're
padding. The synthesis is the product; the raw tiger transcripts are
saved alongside it at `workspace/research/{slug}/tigers/{name}.md` for
anyone who wants to audit a claim.
