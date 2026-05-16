# Tiger Synthesis Rules

How to merge N tiger outputs into one personalized playbook.

## The 5-step synthesis pass

### 1. Read every tiger fully
Don't skim. Each tiger's "couldn't verify" + "surprising findings" sections often contain the most valuable signal.

### 2. Build the agreement matrix
For each MAJOR claim, count which tigers support it:
- **3+ tigers agree** → highest confidence, lead with it
- **2 tigers agree** → confident, present as recommendation
- **1 tiger only** → preserve but flag ("Tiger X surfaced...")
- **Tigers disagree** → ALWAYS surface explicitly, don't smooth over

### 3. Apply user-context weighting
The setup/context tiger's findings are the lens. Generic best-practices that contradict the user's actual constraints get demoted. Example: "use 8 parallel agents" demoted if the setup tiger found the user is on a limited compute tier with a rolling burn limit.

### 4. Surface deltas
For each tiger, document 1-2 things THIS tiger added beyond what others found. This makes the multi-tiger investment legible.

### 5. Preserve uncertainty
The "What I couldn't fully verify" section gets a dedicated heading in the playbook. Don't bury it.

## Synthesis output template

```markdown
# {Topic} — Tiger Research Playbook

## TL;DR
3-5 lines. The bottom line.

## Your profile (from setup/context tigers)
1 paragraph. Specific facts about the user that weight everything below.

## What's cross-validated (high confidence)
3-7 findings ALL/MOST tigers agreed on. Lead with these.

## What each tiger added (deltas)
| Tiger | Unique contribution |
|---|---|
| 🐯 A | ... |
| 🐯 B | ... |
| ...  | ... |

## What's contested
Findings where tigers disagreed. Surface the disagreement, don't pick a side unless you can defend it.

## Concrete recommendations (prioritized)
1. {Action} — {why} — {tiger source}
2. ...

## Anti-patterns to avoid (cross-validated)
What multiple tigers warned against.

## Open questions / professional follow-up
Things tigers couldn't verify. Where the user needs a licensed expert / domain authority / specific data the tigers couldn't access.

## Tiger source attribution
- 🐯 A {name} — {what they researched, what they found of value}
- 🐯 B {name} — {...}
```

## Length guidance

- 2-tiger run → 800-1200 word playbook
- 3-tiger run → 1200-1800 word playbook
- 4-5 tiger run → 1500-2500 word playbook
- 6 tiger run → 2000-3000 word playbook

If the synthesis is shorter than these ranges, you're under-using the tigers. If longer, you're padding.

## When to escalate to human

Surface to the user explicitly when:
- Tigers found a CONFLICT in user-supplied facts (e.g. user said "X" but config shows "Y")
- Tigers found a SECURITY concern in the user's setup
- Tigers found a TIME-SENSITIVE issue (deadline, expiring auth, regulatory window)
- Tigers found a HIGH-DOLLAR risk (legal, tax, financial)

Don't bury these in the recommendations list — call them out in the TL;DR.
