# Spawning Tigers on hermes-agent

[hermes-agent](https://github.com/NousResearch/hermes-agent) has two
spawn primitives. They are not interchangeable, and tiger-research wants
the heavier one.

## Use full subprocess workers, not the delegate tool

| Primitive | What it is | Fit for tigers |
|---|---|---|
| **`delegate` tool** | in-process subagent, `skip_memory=True`, stripped context, bounded by the parent's turn | ❌ no - tigers need full tools/context and must outlive a single turn |
| **`hermes chat -q` subprocess** | a full, independent agent session (its own tools, memory, profile), fire-and-forget, PID-supervised | ✅ yes - this is what the cron/kanban dispatcher already uses for autonomous work |

A tiger is an autonomous, minutes-long, fully-equipped research run.
That is the subprocess-worker shape, not the in-process delegate shape.
Using `delegate` would strip the very tools a tiger needs.

## Durability: surviving gateway interruption

When hermes-agent is fronted by a chat gateway (Telegram, etc.), a new
inbound user message interrupts the current parent turn. Anything
synchronous and tied to that turn dies with it - an in-process
`delegate` child gets cancelled mid-flight and returns a failure like
`Parent agent interrupted - child did not finish in time`. A tiger run
is minutes long and the user will often send follow-up context while it
runs, so this is the common case, not the edge case.

Decision rule:

- **Fast path (`delegate`)** - only for 2-3 short tigers where an
  interruption in the next ~60s is unlikely. Synchronous, simplest, but
  not durable.
- **Durable path (subprocess workers)** - default for anything longer or
  when the user may keep talking. Each tiger is a `hermes chat -q`
  subprocess (or a `hermes cron` one-shot for fully unattended runs), so
  the work is no longer tied to the parent turn and a follow-up message
  cannot cancel it.

When in doubt, take the durable path. The cost of a wasted re-run is
higher than the cost of one extra subprocess.

## Pattern

Spawn each tiger as its own one-shot agent process:

```bash
hermes -p <profile> chat -q "<full tiger prompt - see spawn-claude-code.md template>" &
```

- One process per tiger, launched together, run in parallel.
- Each gets the **self-contained tiger prompt** from
  `spawn-claude-code.md` (role + sources + tools + output shape +
  constraints + time budget). The subprocess does not see the parent
  conversation, so the prompt must stand alone.
- `-q` (quiet/one-shot) makes stdout the tiger's deliverable.
- Capture each tiger's stdout to `workspace/research/{slug}/tigers/{name}.md`.

## Self-contained tiger prompt

The standalone prompt referenced above must include all seven of:

1. Role and mandate (which tiger, what it owns)
2. Exact source types to mine
3. Tools to prefer
4. What the *other* tigers cover, so this one does not overlap
5. Output sections and a word budget
6. Citation requirement (no claim without a source)
7. What to do if evidence is thin (say so explicitly, do not pad)

A prompt missing any of these produces a tiger that overlaps another,
pads thin sections, or cites nothing - all of which waste the parallel
run.

## Orchestration options

- **Ad-hoc:** the orchestrating agent launches the N subprocesses, then
  waits on their output files.
- **Scheduled / unattended:** register the orchestration as a `hermes
  cron` job. A `--no-agent --script` job can fan out the tigers and
  collect results deterministically; non-empty stdout is delivered to
  the configured channel verbatim (same delivery path normal cron
  output uses).

## Capacity

- Respect `agent.max_turns` and the configured model tier per tiger.
- Mix models by tiger weight (cheap model for file/search tigers,
  strong model for domain/critique) - see `spawn-openclaw.md` for the
  per-tiger cost table; the same economics apply.
- Stay within the host's concurrent-session limits; if a spawn is
  rejected for "max concurrent sessions", close stale sessions and
  retry with a smaller batch.

## Delivery

Cron-delivered tiger runs reach the user through hermes-agent's normal
delivery router (e.g. Telegram). Deliver the final synthesis as one
message; optionally announce each tiger's completion if the user wants
live progress.
