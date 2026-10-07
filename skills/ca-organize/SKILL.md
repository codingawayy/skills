---
name: ca-organize
description: How to organize information across files and folders so it stays robust and findable — one home per fact, files that don't point at other files, and one navigation index per folder. Invoke when structuring or restructuring documentation, deciding which file a piece of information belongs in, writing or editing a folder's CLAUDE.md/AGENTS.md index, or context-engineering a directory. The standing rule for how information is arranged across files — as distinct from the wording within any single file.
---

# Organize information so it doesn't rot

Information outlives the moment and the file it's written in. Organize it so a later reader —
usually an AI agent — finds the right thing, trusts it, and never follows a pointer to
something that moved. Two rules do most of the work against rot: **one fact has one home,
and a file points at no other file** (the one navigation index per folder is the single
exception). A third keeps the structure itself legible: **a folder is a coherent parent**,
holding the parts of one concept.

This skill is about *where information lives across files*, not the wording within a file.

## A shared taxonomy

Beneath "one home per fact" and "one folder, one concept" lies the thing they both assume: a
**taxonomy** — the agreed set of categories a domain's things sort into, each thing with one clear
place. It is the map of *what kinds of things exist and where each belongs*; the folder tree
expresses it, and so does the vocabulary people use to point at things.

A taxonomy earns its place by making reference **unambiguous**. When names collide — the same word
naming a concept, a module, and a command — a shared taxonomy resolves each to one thing, because
everyone sorts it the same way; without one, every reference is a guess. So it is not bureaucracy:
it is the backbone both *organization* (where a thing lives) and *communication* (how you name it)
lean on.

A good taxonomy:

- gives each thing **one obvious home** — categories distinct enough that nothing plausibly belongs
  in two, complete enough that nothing falls outside them all;
- is **small and stable at the top** — a few high-level categories that rarely move, deepening only
  where the contents demand;
- **matches how people already think and talk** about the domain, named in its own words — one a
  reader must learn from scratch was never shared;
- **absorbs growth** — new things slot into existing categories instead of forcing a reshuffle.

It does its job only once **written down in one canonical place** anyone — human or agent — can
point to. An undocumented taxonomy lives in one head and can't anchor shared reference; documented,
it becomes the map the whole system names things by.

## Invariants

The properties a well-organized folder always has, at rest.

### One fact, one home

Every fact lives in exactly one file. If two files state it, they drift — pick the home and
delete the other.

Which file is home is decided by scope — and **scope is the question a file answers, not the
subject it covers.** Two files may share a subject while answering *different questions* about
it: rationale vs. mechanics, overview vs. detail, one audience's version vs. another's. They
duplicate only when they answer the *same* question. The trap is an overview that restates what the
detailed file explains, instead of just naming it — a split that has stopped doing its job. So
when a fact seems to want two homes, the boundary is wrong, not the fact: re-scope until the urge
to copy disappears.

Scope has altitude as well as shape: a fact's home is the **smallest subtree whose readers need
it** — system-wide truth at the root, an area's detail in that area. A reader resolves a fact by
checking their own folder first, then ascending through its parents. So the deeper a reader sits,
the more they see: their local facts plus every system-wide fact above them.

Coined terms are facts too: each gets one home — a glossary, when the repo has jargon worth
pinning down.

### One folder, one concept

A folder names a concept, and its direct children — files and subfolders alike — are the parts
of it: siblings at one level of abstraction. This is **cohesion** — everything in a folder
belongs to the folder and to each other. A folder you can only describe as a *list* ("source,
tests, config, the build scripts…"), never as a concept whose parts these are, is a **bag**: a place
things were dropped, not a parent.

Variety alone isn't the smell — every folder's top level is varied. A bag is revealed by a child
that doesn't belong where it sits:

- **a child with a truer home among its siblings** — a web build's config sitting at the repo
  root when it belongs with the web app;
- **a child that belongs with nothing here** — neither part of the folder's concept nor a fit
  under any sibling, so it just sits in the folder.

The fix is never to widen the folder's description until the stray fits. It's to introduce an
**intermediate parent** that genuinely parents its contents, and move the child under it.

**Findability is the counterweight.** Cohesion pushed to the limit buries things: a parent
holding a single child, or a chain nested so deep its contents are hard to reach, trades one
failure for another. Stop short — don't manufacture a parent for one thing, don't nest past what
a reader can comfortably walk. When the two pull against each other, that tension is a judgement
call to surface, not to resolve by reflex.

The tactical calls this raises — where a tool's config belongs, when a folder should become its
own package, how source and tests sit — turn on how tools find their files; an applied guideline
covers them.

### Documents are timeless

A document states what *is*, today. So **temporal information is a red flag**: ask of any sentence
whether it would change as time passes or as work advances — if it would, it's temporal, and its
home is a system built for time, not prose. Three forms recur, each with a home that is *not* a
doc:

- **Past** — an audit log, changelog, "what shipped" note → version control already holds it.
- **Future** — a roadmap, projection, plan of what's coming → the tracker (issues, a backlog).
- **Progress** — how far along a thing is → the system that owns the work (a tracker, a status field).
- **Provenance** — *which* change or work item produced a thing (a commit hash, or a backlog-item ID
  tagged into a comment or doc) → git blame and the tracker already hold that link. Explain what the
  thing *is*, in the prose's own words; don't cite the work that introduced it.

**The exception is content whose job is time.** Some folders exist precisely to hold temporal files
— a changelog, an ADR/decision log, a status board's data — and a single **charter or vision**
document exists to chart direction and what's coming. There the temporal content is the point; the
red flag is the *general* reference or how-it-works document that smuggles in history, roadmap,
progress, or provenance it has no business owning.

### A file points at no other file

A reference — any path or filename naming another file in the repo — is a dependency that
rots: move the target and the pointer lies. So a content file names no other file. To lean on
another file, name it **conceptually** — "the schema source," "the build config," "the API spec" —
which survives a rename and which a reader finds by looking. That also keeps one home per
fact: you bind to what the file *is*, not its path or its content.

- **The test:** naming a *file* (a path or filename) is a reference — cut it; naming an *idea
  or subject* is fine — keep it.
- **Outside the repo is different.** Name a genuinely external thing by identifier or title — "the
  OAuth 2.0 spec," an upstream library's issue. A raw URL earns its place only when the URL itself is
  the canonical artifact (a docs site, a standard). But the project's *own* backlog-item IDs are
  **provenance, not a reference target** (see "Documents are timeless") — keep them out of code and prose.

The one exception is the index.

### One index per folder

Navigation is a real need, so it gets one home too — but as a *part* of one file, not a file of
its own. A folder's **agent instruction file** (the file an agent loads automatically) carries an
**index**: a table of contents listing the folder's files and immediate subfolders. That index is
the one place file names are allowed — the single exception to the no-pointers rule; the
instruction file's content, its standing instructions, names none.

- **Never a second index.** A folder's table of contents has one home in its instruction file; a
  second file that re-lists the folder duplicates it — fold them into one.
- **A folder earns an instruction file only when it has enough to index** — about three files, or
  a subfolder it hands off to rather than lists inline. A one- or two-file folder needs none.

The same instruction file is **both** index and content: the repo-root one lists the top-level
folders *and* carries the standing instructions an agent needs each session — conventions, how to
run things — that have no smaller folder to live in. A leaf folder's single substantial document is
likewise both. Each role keeps its rule: the index names files; the content names none.

#### Which file carries the index

`AGENTS.md` is the cross-tool instruction file — Codex, Cursor, Copilot and most other agents read
it. Claude Code reads `CLAUDE.md` instead. A repo used with several agents keeps **one** of them as
the home and bridges the other to it, never two copies. A skill's `SKILL.md` is the index-bearer
for its own folder, listing its `guidelines/` the same way. A `README.md` survives only for
genuinely human-only content — self-contained, and it never carries the index.

#### Hand off or inline a subfolder

An index covers its own folder's level — the files in it and its immediate subfolders — and
usually stops there, handing a subfolder's *contents* off to that subfolder's own instruction file.
Whether to hand off or inline is a judgement call under one rule: **each subfolder's listing has a
single home — its own instruction file or the parent's index inline, never both.** Give a subfolder
its own instruction file when it's a major piece a reader enters deliberately (its own audience and
concerns, loaded as a unit), or when inlining it would push the parent index past scannable. Inline
it instead — grouped under a heading — when it's a lightweight grouping of short entries that just
keeps the parent legible, like a glossary's sub-domains. Default to handoff; reach in only when the
extra hop costs the reader more than it saves.

### Stay findable

A reader has to be able to find what's in a folder, and they meet it two ways — either of which can
fail, and either failure is the signal that the scheme has been outgrown:

- **the index** (when the folder has one) — its table of contents points you to the right file
  fast; it fails when it grows so long or flat that it no longer guides.
- **the bare names** — the files and subfolders read directly, with no index, tell you what's
  where. This is how a person first meets a folder, so it matters even when the index is fine. It
  fails when the folder is too crowded to take in by eye — so watch size, but as a *cause* of lost
  findability, not the signal itself.

## Restructuring

When a folder has outgrown its scheme, the fix is rarely local. Splitting the biggest folder or
tucking strays into a new subfolder just pushes the same scheme down a level. Step back and
rethink the whole subtree: read everything in it, build a picture, then ask what structure the
contents now want. That
structure may organize along entirely different *dimensions* — by audience where it was by
topic, by lifecycle where it was by type — and look nothing like what it replaced. That's the
point: re-derive from the contents as they are now, don't patch the scheme that no longer fits.

Because a restructuring this size moves and renames many files at once, it's a deliberate
whole-tree exercise: surface it as a proposal, and fix what it disturbs with a rename's care —
never a silent local edit.

## Guidelines

Specific organizing techniques that apply these principles live as guidelines — read the one
that fits your task:

```
ca-organize/
└── guidelines/       # applied techniques, one per file
```

Each guideline starts with a `when:` line in its frontmatter. Search this skill's `guidelines/`
folder for lines that start with `when:`, then read the guideline whose `when:` matches your task.
