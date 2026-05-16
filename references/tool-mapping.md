# Tiger Tool Mapping

Which research tools each tiger archetype should prefer. The point of a
tiger grid is *non-overlapping tool preferences* — if two tigers reach
for the same tool on the same source, you've wasted the parallelism.

| Tiger | Primary tools | Secondary | Explicitly avoid |
|---|---|---|---|
| 🐯 Docs | `WebFetch` official docs, `Read`/`Grep` on the codebase | docs-index / Context7-style | community opinion, speculation |
| 🐯 Setup | `Read` the user's config files, `Bash` for live state | local logs | the open web (that's other tigers) |
| 🐯 Community | X/Twitter search, web search, forum/Discord threads | `WebFetch` blog posts | official docs (that's the Docs tiger) |
| 🐯 Domain | deep web research (high reasoning effort), authoritative crawling | citation chasing | the user's config (that's Setup) |
| 🐯 Critique | all tools, **adversarial framing** | precedent lookup | being agreeable — its job is to find gaps |
| 🐯 Pattern | code/repo search, blog/postmortem discovery, crawling | GitHub search | generic best-practice prose |
| 🐯 Project Context | `Read` the user's notes/projects/daily logs | architecture docs | the open web |

## Principles

1. **One source type per tiger.** Docs ≠ community ≠ user-files ≠
   authoritative-domain ≠ patterns ≠ adversarial. Assign each tiger
   exactly one and tell it what the others cover so it doesn't poach.

2. **Match tool cost to tiger value.** File-reading and search tigers
   (Setup, Docs, Community, Pattern) can run on a cheap/fast model.
   Reasoning-heavy tigers (Domain, Critique) deserve the strong model.
   Mixing models per tiger is the main cost lever — see
   `templates/spawn-openclaw.md`.

3. **Tools are a preference, not a cage.** A tiger may step outside its
   primary tool if a thread demands it — but it must not duplicate
   another tiger's *mandate*. Overlap of a single lookup is fine;
   overlap of scope is waste.

4. **Citations are non-negotiable regardless of tool.** Every claim
   carries its source URL/path. A tiger that can't cite a claim must
   label it "unverified", not drop it and not assert it.
