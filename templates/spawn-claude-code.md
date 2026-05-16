# Spawning Tigers in Claude Code

When invoked from Claude Code, use the `Agent` tool with `subagent_type: "general-purpose"` and `run_in_background: true`.

**Always spawn all tigers in ONE message with multiple tool calls** (parallel execution).

## Template

```
Agent({
  description: "Tiger A: docs research",
  subagent_type: "general-purpose",
  prompt: "<full tiger prompt — see archetype templates>",
  run_in_background: true
})

Agent({
  description: "Tiger B: setup analysis",
  subagent_type: "general-purpose",
  prompt: "<full tiger prompt>",
  run_in_background: true
})

# ... etc, all in the same message
```

## Tiger prompt structure

Each tiger gets a self-contained prompt. The general-purpose subagent doesn't see the parent conversation, so the prompt must include:

1. **Role + mandate** — "You are research tiger #N doing X"
2. **Sources to read** — exact file paths, URLs, search queries
3. **Tools to prefer** — explicit list, with examples
4. **Output structure** — exact sections + word counts
5. **Constraints** — citation rules, length limits, what to skip
6. **Time budget** — explicit max wall-clock

## Standard tiger prompt template

```
You are research tiger #N in a parallel research swarm. Your specific mandate: {ONE_LINE_MANDATE}

**Tools you should use:**
{TOOL_LIST_WITH_EXAMPLES}

**Sources to mine:**
{SOURCE_LIST_WITH_EXACT_PATHS}

**What to extract:**
{EXTRACTION_GOALS}

**Output structure (under {WORD_COUNT} words):**

### Section 1: {NAME}
{WHAT_TO_INCLUDE}

### Section 2: {NAME}
{...}

### Section N: What I couldn't verify
Be honest. Which queries returned nothing useful. Don't pad with generic advice.

**Constraints:**
- Cite source URLs/paths for every claim
- Quote selectively (1-2 short quotes max)
- {DOMAIN_SPECIFIC_RULES}
- {TIME_BUDGET} total budget — don't spelunk indefinitely

**Don't repeat work other tigers are doing:** {WHAT_OTHER_TIGERS_COVER}
```

## Waiting for tigers

After spawning, the system pushes completion notifications. Don't poll.

Use the wait time to:
- Prepare the synthesis template at `workspace/research/{slug}/playbook.md`
- Re-read the user's original question for cross-validation hooks
- Build a list of contradictions to look for in the outputs

## When tigers complete

Each tiger's output appears as a notification. Read each fully before synthesizing.

If a tiger times out or fails:
- Read its partial transcript at the background-task output path the runtime reports for that agent
- Decide: was its mandate covered by another tiger? If yes, proceed. If no, spawn a replacement with tighter scope.
