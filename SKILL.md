---
name: tiger-research
version: 1.0.0
description: Parallel multi-source deep research via "research tigers" — spawn N specialized research agents in parallel, each using different tools/perspectives, then synthesize their findings into a single personalized playbook. Use when the user wants a "deep dive", "tiger research", "perspective check", "pull every source", or asks for thorough research that benefits from multiple angles (legal/tax review, technical architecture analysis, document critique, project audit, parallel tool evaluation, anything where one-shot research misses important angles). Outputs a synthesized playbook saved to workspace/research/{slug}/playbook.md plus per-tiger raw transcripts.
metadata: {"emoji":"🐯","requires":{"bins":["jq"]}}
---

# 🐯 Tiger Research

Parallel multi-source deep research via specialized "tigers" — each tiger is a research agent with a specific mandate, scope, and tool preference. Spawn them in parallel, wait for all, synthesize cross-validated findings into one personalized playbook.

## When to use

Trigger on phrases like:
- "tiger research on X"
- "deep dive on X"
- "pull every source for X"
- "let the tigers loose on X"
- "perspective check on X"
- "research tiger for X"
- Or any time the user wants thorough research that benefits from MULTIPLE independent angles (not one-shot)

**Don't use for:** simple lookups, single-fact verification, quick web searches. Tigers are for problems where one perspective misses something.

## Core philosophy

1. **Multiple independent angles beat one thorough one.** A single research pass has bias toward the first angle it finds. Multiple tigers attacking from different vectors surface contradictions and validate findings.

2. **Each tiger gets a focused mandate + non-overlapping scope.** Tiger A reads docs, Tiger B reads the user's actual config, Tiger C reads community, Tiger D reads adjacent context. They don't duplicate work.

3. **Personalization is the point.** The synthesis cross-references the user's actual situation (config files, project portfolio, constraints) — not generic best-practice advice.

4. **Honesty about confidence.** Each tiger flags what it couldn't verify. The synthesis preserves uncertainty rather than smoothing it over.

## Tiger archetypes

Choose 2-6 tigers per research run depending on scope. Common combinations below.

### 🐯 Docs Tiger
**Tools:** WebFetch, Read, Grep on official docs/codebase
**Use for:** "what does this software/system officially support"
**Output shape:** Architecture overview + N concrete patterns + commands inventory + anti-patterns

### 🐯 Setup Tiger
**Tools:** Read the user's own config files, Bash for state inspection
**Use for:** "what does the user actually have running"
**Output shape:** Engagement profile + active config + capacity calculator + underused features + cited paths to the user's real files

### 🐯 Community Tiger
**Tools:** X/Twitter search, web search, WebFetch
**Use for:** "what do power users actually do (not what docs say)"
**Output shape:** Quoted snippets with attribution + capacity numbers from real users + community-validated patterns + anti-patterns called out by name

### 🐯 Domain Tiger
**Tools:** Deep web research (high reasoning effort), authoritative-source crawling
**Use for:** Topics that need authoritative cross-source synthesis (legal, tax, regulatory, scientific)
**Output shape:** Authoritative citations + jurisdiction-specific facts + verdict matrix + what couldn't be verified

### 🐯 Critique Tiger
**Tools:** All research tools + adversarial framing
**Use for:** Reviewing a user-generated artifact (memo, contract, deck, proposal) — finds gaps the user missed
**Output shape:** Reality check + verdict matrix per dimension + concrete clause/text proposals + professional follow-up questions

### 🐯 Pattern Tiger
**Tools:** Web crawling, code/repo search, blog-post discovery
**Use for:** "find people who solved this and what their config/code looked like"
**Output shape:** Reverse-engineered configs + real-world bottleneck reports + anti-patterns from postmortems

### 🐯 Project Context Tiger
**Tools:** Read the user's notes/projects, daily logs, architecture docs
**Use for:** Building a portfolio picture before another tiger goes deep on one project
**Output shape:** Active project portfolio + cross-cutting threads + trajectory + surprising findings

## Workflow

### Step 1: Scope clarification (skip if the user already specified)

Ask up to 4 questions max — only the essentials:
1. **Topic/artifact** — what are we researching?
2. **Output target** — playbook? critique? recommendation? decision support?
3. **Personal context** — anything specific about their situation that should weight the synthesis?
4. **Time budget** — quick (5-10 min, 2-3 tigers) or deep (20-40 min, 4-6 tigers)?

If the invocation already answers these, skip — just plan the grid.

### Step 2: Plan the tiger grid

Pick 2-6 tigers from archetypes. Make them **non-overlapping** in scope:
- Each tiger gets a different SOURCE TYPE
- Each tiger gets a different TOOL PREFERENCE
- Each tiger gets a focused output shape

Document the plan briefly to the user before spawning ("I'm spawning 4 tigers: A docs/codebase, B your setup, C community signals, D project context").

### Step 3: Spawn in parallel

**In Claude Code:** use the `Agent` tool with `subagent_type: "general-purpose"` and `run_in_background: true`. Spawn all tigers in ONE message with multiple tool calls (see `templates/spawn-claude-code.md`).

**In an OpenClaw-style multi-agent host:** use the host's session-spawn tool. Each child gets its own fresh session, no main-context pollution. (See `templates/spawn-openclaw.md`.)

**Capacity guidance:**
- Max ~5 parallel tigers on most auth tiers
- Max 6 in burst — but watch for rate limits
- For longer-running tigers, use a lower-tier model where possible

### Step 4: Wait without polling

Tigers complete asynchronously. Don't poll their status — the system pushes completion notifications. While waiting, use the time for:
- Preparing the synthesis template
- Reading any prior context the user gave
- Building a list of cross-validation questions to apply once results arrive

### Step 5: Synthesize

When all tigers return:

1. **Read every tiger's output**
2. **Cross-validate** — find points where 2+ tigers agree (highest confidence) or disagree (flag explicitly)
3. **Personalize** — weight findings by what the setup tiger surfaced about the user's actual situation
4. **Surface deltas** — highlight what each tiger added beyond the others
5. **Flag uncertainty** — preserve "what couldn't be verified" honestly

Use the synthesis structure from `references/synthesis-rules.md`.

### Step 6: Save the playbook

Write the final synthesis to `workspace/research/{slug}/playbook.md`. Save tiger raw outputs to `workspace/research/{slug}/tigers/{tiger-name}.md`. The slug is a kebab-case version of the topic (e.g. `equity-term-sheet-critique`, `agent-parallelism`, `multi-tenant-architecture`).

## Constraints

- **No tiger gets more than 30 minutes wall-clock budget.** Stuck tigers are killed.
- **Cite sources for every claim.** No "research suggests" without URLs.
- **Quote selectively** — 1-2 short quotes max per tiger if a phrase is illuminating.
- **No padding.** If a section has thin evidence, say so explicitly.
- **Match output to context.** Don't generate 1500-word reports for 5-min lookups.
- **Preserve user voice.** If the user has a specific tax residency / stack / constraint, address those specifics; don't generic-out.

## Examples

### Example 1: "tiger research on parallelism for my agent setup"
**Tigers spawned:** Docs (official docs + codebase) / Setup (the user's config + scheduler + sessions) / Community (power-user threads on parallel agent patterns) / Project Context (active projects that would benefit from parallelism)
**Output:** Personalized parallelism playbook with a capacity calculator + concrete patterns matched to the user's actual project portfolio.

### Example 2: "tiger research on this equity term sheet I received — focus on the holding structure"
**Tigers spawned:** Critique (adversarial legal review of the document) / Domain (relevant tax/jurisdiction law, treaty + reporting obligations) / Setup (the user's residency / entity / prior-history facts) / Pattern (comparable cross-border equity-holding structures from public sources)
**Output:** Verdict matrix + concrete clause language + jurisdiction red flags + recommendation + open questions for a licensed professional.

### Example 3: "deep dive on optimal newsletter cadence for a niche e-commerce brand"
**Tigers spawned:** Domain (newsletter best-practice research) / Pattern (comparable niche brands' actual cadences from publicly visible archives) / Setup (the user's existing email infra, list size, engagement data)
**Output:** Cadence recommendation matched to list state + competitor benchmark + content-type rotation suggestion.

## Output structure

Synthesis saved as `workspace/research/{slug}/playbook.md` with:

1. **TL;DR** (3-5 lines, the bottom line)
2. **Personalized profile** (1 paragraph from setup/context tigers)
3. **What's cross-validated** (multiple tigers agreed)
4. **What's contested** (tigers disagreed — explicit flagging)
5. **Concrete recommendations** (numbered, prioritized)
6. **Open questions / professional follow-up needed**
7. **Tiger source attribution** (which tiger contributed what)

See `templates/synthesis-output.md` for the full playbook template.

## Files in this skill

- `references/synthesis-rules.md` — How to merge tiger findings into one playbook
- `references/grid-templates.md` — Pre-built tiger combinations for common topic types
- `references/tool-mapping.md` — Which research tools each tiger should prefer
- `templates/spawn-claude-code.md` — Spawn syntax for Claude Code
- `templates/spawn-openclaw.md` — Spawn syntax for an OpenClaw-style host
- `templates/synthesis-output.md` — Final playbook template

## Anti-patterns

- ❌ **Don't spawn 1 tiger** — that's just regular research. Tigers need ≥2 to triangulate.
- ❌ **Don't spawn overlapping tigers** — two tigers reading the same docs are wasted parallelism. Each tiger gets a different SOURCE TYPE.
- ❌ **Don't poll tiger status** — completion is push-based. Polling burns context budget.
- ❌ **Don't synthesize without reading every tiger output** — missing one tiger's surprise finding defeats the purpose.
- ❌ **Don't generalize the user** — "you're a busy entrepreneur" is useless. Use specific facts from setup tigers (e.g. "your scheduler's max-concurrent is 1, bottlenecking your N scheduled jobs").
- ❌ **Don't hide uncertainty** — if 3 tigers couldn't verify a claim, say "couldn't verify" not "research suggests".
