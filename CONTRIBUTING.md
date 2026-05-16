# Contributing

tiger-research is a **methodology skill**, not code - it's prompt and
process. Contributions are improvements to the method, the archetypes,
the grids, or the synthesis discipline.

## Good contributions

- A new **tiger archetype** with a clear, non-overlapping mandate and a
  defined output shape (add to `SKILL.md` + `references/tool-mapping.md`).
- A new **grid template** for a recurring research shape
  (`references/grid-templates.md`).
- Sharper **synthesis rules** - especially anything that improves
  cross-validation or stops uncertainty getting smoothed over.
- A host adapter (`templates/spawn-*.md`) for another multi-agent
  runtime.

## Principles to preserve (don't regress these)

- **Non-overlapping scope.** Two tigers with the same source type is
  wasted parallelism. Every archetype must own a distinct SOURCE TYPE.
- **Honesty about confidence.** "Couldn't verify" is a first-class
  output, never dropped and never upgraded to a claim.
- **Personalization is the point.** The synthesis weights findings by
  the user's actual situation; generic best-practice that contradicts
  their constraints gets demoted.
- **No padding.** Length follows evidence, not the other way around.

## Style

Keep it tool-agnostic where possible. The skill should work on any
host that can spawn parallel agents; host-specific detail belongs in
`templates/spawn-<host>.md`, not in `SKILL.md`.

No em dashes in prose. Plain, direct, example-led.
