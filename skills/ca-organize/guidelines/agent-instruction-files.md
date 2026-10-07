---
when: organizing agent instruction files (AGENTS.md, CLAUDE.md, and similar) — deciding where agent-facing instructions live, how they load (hierarchy, imports, path-scoped rules), and how to keep always-loaded context lean
---

# Organizing agent instruction files

**The reader is a context window.** Every line in an always-loaded file is paid for on every
task, whether or not it's relevant. So these files carry a budget no ordinary folder does: get
the right instruction in front of the agent when it's working *here*, without paying for it when
it's working *elsewhere*.

The file name depends on the agent: `AGENTS.md` for Codex, Cursor, Copilot and most others;
`CLAUDE.md` for Claude Code. The principles below hold for all of them. The loading mechanics
differ per agent — check your agent's documentation before you design around them.

## How these files load

An agent doesn't read these files the way a person browses a folder. The tool loads them by
position, and *where a file sits decides when it loads*:

- **Ancestors load in full, at launch.** Most agents concatenate the user-level file and the
  files from the repo root down to the working directory into context the moment they start.
  They're additive — stacked root-first, nearest-last.
- **Subdirectory files may load on demand.** In Claude Code, a file *below* the working directory
  loads only when the agent touches a file in that directory. Not every agent does this; some read
  only the chain down to where the session started.

So a fact's home is the smallest subtree whose readers need it: put repo-wide rules at the root,
an area's conventions in that area's own file, and the agent loads each only where it applies. The
reward is paid in tokens.

## When to add a nested file

An instruction file carries both an **index** — the table of contents naming the files in its
folder — and its **content**, the standing instructions. That hybrid decides the hierarchy:

- **Add a nested instruction file when a subtree earns its own** — a major piece an agent enters
  deliberately, with its own stack of conventions. A lightweight grouping doesn't get one; its
  conventions stay in the parent.
- **One per folder, never two.** Don't pair an instruction file with a second one covering the
  same scope — except the bridge described below, which holds no content of its own.
- **The index may name files; the content prose names none.**

The anti-pattern is a **single monolithic root file** that grows to cover every subsystem — paying
always-loaded context for instructions irrelevant to the task at hand, or staying so generic it
doesn't help. When the root file stops being scannable, push area-specific facts down to nested
files.

## Splitting: organize or save context

Two different reasons to split a file, with two different mechanisms — don't confuse them:

- **Imports organize; they do not save context.** Some agents let an instruction file pull in
  another file (Claude Code: `@path/to/file`). The imported bytes are **expanded into context at
  launch**, same as if they were inline. Use an import to keep one logical document readable, not
  to shrink the budget.
- **Conditional loading saves context.** Some agents can load a rule only when the agent works on
  matching files (Claude Code: a rule under `.claude/rules/` with a `paths:` glob; Cursor: rules
  with globs). That conditional load is what trims always-loaded context. An *unconditional* rule
  file is just organization, like an import.

So the real dividing line is **always-loaded vs. on-demand**, not file-vs-directory. Imports and
unconditional rules sit on the always-loaded side; on-demand nested files and conditional rules sit
on the on-demand side. When a body is large or only sometimes relevant, the move is to get it onto
the on-demand side. At the far end, heavy procedural content can leave these files entirely for a
skill, which loads only when invoked.

## Imports are includes, not pointers

A content file names no other file's path, because a prose pointer rots when the target moves. An
import (or a symlink) names a literal path too — but it's a **mechanical include**, not a pointer:
it pulls the target's content *into* this file instead of sending a reader off to find it. That's
the difference that makes it allowed.

It's still a real rename-dependency. So keep these few, and fix the import whenever you move or
rename its target. Everywhere else, name things conceptually, not by path.

## Bridging AGENTS.md and CLAUDE.md

A repo used with several agents needs both files — and two copies drift. Make `AGENTS.md` the home,
since most agents read it. Then bridge `CLAUDE.md` to it: a `CLAUDE.md` that holds only the line
`@AGENTS.md`, or a symlink (prefer the import on Windows, where symlinks need elevated
permissions). Every agent then reads the same bytes.

## Size is a symptom, not a rule

An over-long always-loaded file is a symptom, not a line-count violation. Current guidance puts
the soft target around a couple hundred lines per file — past that, adherence drops and you're
likely paying for facts that belong in a nested or conditional home. Use the number as
corroboration; act on the real signal: *the file stopped being scannable, so push facts down to
where they're needed.*

## Don't rely on conflict precedence

When two layered files genuinely contradict, which one "wins" differs between agents and is not
reliably defined. Don't design around precedence — give each altitude its own facts so layers
complement rather than collide.
