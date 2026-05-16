# Spawning Tigers in an OpenClaw-style host

When invoked from an OpenClaw-style multi-agent host, use the host's
session-spawn tool (e.g. `sessions_spawn`).

## Capacity

Typical multi-agent-host defaults:
- a global subagent concurrency lane (e.g. `maxConcurrent: 8`)
- a per-parent child cap (e.g. `maxChildrenPerAgent: 5`)
- a spawn-depth limit (often `1` by default) — set to `2` for the
  orchestrator-pattern tiger grids

For tiger-research, raise the spawn-depth limit to `2` in the host
config if it isn't already.

## Spawn syntax

```
sessions_spawn({
  agentId: "main",                 // or a specialized agent if defined
  task: "<full tiger prompt — see spawn-claude-code.md template>",
  model: "<strong-model>",         // or a cheaper model for light tigers
  thinking: "high",                // for domain/critique tigers
  metadata: {
    role: "tiger-research",
    tigerName: "docs|setup|community|domain|critique|pattern|project-context",
    parentSlug: "{topic-slug}"
  }
})
```

Spawn all tigers in ONE turn (multiple tool calls in the same response). They run in parallel on the subagent lane.

## Tracking

Each child writes its trajectory to the host's per-session trajectory
log. The main session gets push notifications when each child completes
(no polling needed).

## Cost guidance for rolling-burn compute tiers

Tigers on a premium model consume the rolling compute window. Heuristics:
- 4 concurrent tigers running ~5 min each ≈ a small slice of the window
- 6 concurrent tigers running ~10 min each ≈ a noticeable slice
- For routine tiger-research, prefer a cheap/API-billed model for
  non-strategic tigers, and the premium model only for
  synthesis-critical tigers

Mixing models per tiger:
- Setup Tiger → cheap — just reads files
- Docs Tiger → cheap — just reads docs
- Community Tiger → cheap — just searches
- Domain Tiger → expensive — needs deep reasoning
- Critique Tiger → expensive — adversarial reasoning
- Pattern Tiger → cheap — just searches

This optimizes burn while keeping reasoning-heavy work on the strong model.

## Delivery (optional)

If the user wants to see tiger progress without watching logs, use the
host's delivery/announce option so each tiger announces its completion
directly to the user's chat channel, and deliver the synthesis as a
separate message.

## When a tiger fails

If spawn fails with a "max concurrent sessions reached" error → idle
sessions are blocking; close stale ones and retry.

If a tiger hangs (no trajectory updates for >5 min while expected to be
active):
- Check the host's worker process load
- If stuck → kill via the host's subagent-kill command
- Spawn a replacement with shorter scope
