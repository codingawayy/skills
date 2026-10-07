---
name: ca-challenge
description: Run an adversarial panel of independent agents against a proposal to widen the lens — annoying colleagues who interrogate every hand-wave, chaos monkeys who propose wild alternatives from other domains, and specialists who attack the premise, the prior art, the failure modes, and the blind spots. Every panelist grounds its claims with live web search rather than memory. Invoke when the user runs /ca-challenge, or asks to stress-test, poke holes in, red-team, steelman the alternatives to, or challenge the assumptions behind a plan, design, or direction — especially one this session already settled on.
---

# Challenging a proposal from every direction

A model that has settled on a direction stops seeing alternatives to it. Every
later thought is spent elaborating the choice rather than questioning it, and the
elaboration reads as confidence. This skill buys back the lost range: a panel of
independent agents, each with a **different axis of attack** and no attachment to
the current plan, poking at the proposal until the premise itself is on the table.

The output is not a verdict. It is a widened option space and a short list of
questions that must be answered before proceeding.

```
ca-challenge/
└── reference/panel.md    # the persona roster and what each one is for
```

## Two things that make this work

**Diversity beats rigor.** This is not a code review, and the panel is not a
verification pass. Do not filter challenges down to the ones that survive
adversarial scrutiny — the most valuable output is often the wild suggestion that
would fail a feasibility check but reframes the problem. Rank by _would this
change the plan_, never by _is this comfortable_.

**Grounding beats recall.** Every panelist must use web search and page fetch.
A challenge built on a remembered fact about a library, a price, an API, or a
competitor is worse than no challenge — it manufactures false confidence in a new
direction. Panelists must mark every claim as `[verified: <url>, <date>]` or
`[suspicion]`, and never blur the two. If your agent has no web access, every
claim is a `[suspicion]` — say so at the top of the result.

## Procedure

1. **Write the brief first.** The panel cannot see this conversation, so capture
   the target in `wip/{yyyy.MM.dd}_{short-task}/challenge-brief.md` (or the
   repo's scratch convention): what is being proposed, why, what has already
   been decided and on what grounds, what the constraints are, and what is
   explicitly out of scope. State the proposal as its advocate would — a
   weak brief produces a strawman panel. If the user names a document or an
   item, read it; if they name nothing, the target is whatever the session just
   converged on, and you must say back in one line what you understood the
   proposal to be before dispatching.

2. **Pick the panel.** Choose from `reference/panel.md`. Default is **7
   personas**, always including at least one from each ring. Trivial or narrow
   proposal → 4. "Deep", "everything", or a decision that is expensive to
   reverse → the full roster plus a second round seeded with round one's
   synthesis. Never fewer than 3; a two-person panel is a coin toss, not a lens.

3. **Run the panel.** Shape and fallbacks below. Invoking this skill is the
   user's opt-in to running several agents.

4. **Present a decision, not a transcript.** The panel will return more material
   than is useful. Lead with the three challenges that would most change the
   plan, then the assumptions now in doubt, then the alternatives worth a second
   look — and end with your own honest read: what you would now change, what you
   would defend, and what you cannot answer without the user. Link the full
   synthesis file rather than pasting it.

## Panel shape

Three stages, in order:

1. **Challenge** — every persona runs in parallel, in its own fresh context. Each
   reads the brief, does its own searching, and returns its challenges. Separate
   contexts matter: a panel that shares one framing converges on one objection.
2. **Synthesize** — one agent reads every challenge at once. It clusters them,
   drops exact duplicates (keeps near-duplicates from different personas —
   independent arrival is signal), ranks by how much each would change the plan,
   and keeps the wild ideas. It writes the synthesis file. Use high effort here.
3. **Blind spots** — one agent reads the brief and the synthesis and asks what
   class of concern the whole panel missed (the `blind-spot-mapper` persona).

Use the strongest machinery your agent has:

- **Parallel subagents** — best. Dispatch every persona in one message so they run at
  once. Give each a distinct output file. Then run synthesis and blind spots as
  two more subagents, in sequence.
- **No subagents** — run each persona as a separate session if the user is
  willing to start them; otherwise run each inline as its own pass, writing its
  own file before the next starts, and do not reread earlier passes. Say in the
  result that the panel was not independent, because one context played every
  role.

Each persona prompt is its seed from `reference/panel.md`, sharpened against
the actual proposal, plus the rules for panelists below and today's date (so
the panelist knows its memory is stale).

### What each challenge returns

- `persona`
- `challenge` — one sentence
- `attacks` — `premise` | `approach` | `consequences` | `omission`
- `grounding` — the verified claims and their URLs, or empty
- `ifTrue` — what changes about the plan
- `confidence` — `verified` | `argued` | `provocation`

Provocations are first-class — the chaos monkey's output is supposed to land
there.

## Rules for panelists

Carry these into every persona prompt.

- **Do not validate.** No panelist is permitted to conclude the proposal is fine.
  If a persona genuinely finds nothing on its axis, it says so in one line and
  spends its effort on the strongest challenge it can construct anyway.
- **Attack the load-bearing part.** A challenge to a detail the plan does not
  rest on is noise. Find the assumption that, if wrong, makes the rest moot.
- **One angle only.** Stay in your lane; overlap across personas is handled at
  synthesis, and a panelist that covers everything covers nothing.
- **Name what would settle it.** Every challenge ends with the evidence,
  experiment, or answer that would resolve it — otherwise it is just doubt.
- **Return your sharpest challenges, not all of them.**
