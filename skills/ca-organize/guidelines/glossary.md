---
when: creating or maintaining a glossary of domain terms — deciding where a coined term is defined, or growing a glossary that has sprawled
---

# Maintaining a glossary

A glossary is the home for a repo's coined vocabulary, and it exists because the "name things
conceptually" rule only works if every reader resolves a name the same way. Reach for one when
a repo has domain jargon a reader could misread; a repo whose terms are all ordinary needs none.

## What earns an entry

Each term this repo coins or gives a special meaning (`tenant`, `saga`, `reaper`, `nonce`) is
defined once, in a glossary. A general term that exists outside the repo (`SaaS`, `SQLite`) gets
none — the test is whether a competent newcomer would misread it without a project-specific
definition.

## Where a term is defined

Vocabulary layers like everything else, so a reader loads only what their altitude needs:

- **A term is defined at the smallest subtree whose readers need it.** Used inside one module →
  that module's glossary (`nonce` → the auth module's); needed to understand the system at all →
  the root glossary (`tenant`, `saga`); used by two modules → the glossary they share, usually the
  root. A reader resolves a term by checking their own folder's glossary, then ascending.
- **Never twice.** The same term in two sibling glossaries proves it belongs in their parent —
  move it up, delete the copies.
- **A folder earns a glossary only when it coins terms of its own.** If everything it uses is
  defined higher, it has none and uses those terms by name.

## The glossary as a folder

A glossary is a folder — one file per concept, each from a sentence to a page — living in the
subtree whose readers need its terms. *Where* it sits within that subtree follows cohesion like
anything else: place it beside the other things of its kind — under a shared reading or reference
parent, if the subtree has one — rather than defaulting to the subtree root. Its index is the
scannable listing: every term with a one-line gloss, the full definition one hop away.

## When it grows

Findability governs a glossary too, both faces applying: the index can
sprawl until it stops guiding, or the flat folder of term-files can grow too long to browse.
Group the terms into sub-domains once natural clusters emerge — as labeled sections in the one
index, or, when the bare folder itself grows too long, as subfolders. A glossary's sub-domains are
display buckets, not subtrees a reader enters, so they stay indexed inline in the single index,
grouped by heading — they don't each earn an index. Either way it's a re-derivation of the
glossary's shape, not a move of terms between subtrees (still governed by the placement rule
above).
