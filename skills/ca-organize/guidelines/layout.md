---
when: deciding where a file goes when something beyond its concept fixes the location — a config or manifest pinned by how a tool finds it, a cluster of files that should become its own folder, or where source and tests sit
---

# Laying out files

## Place a file with what it governs

A file lives with what it governs — configs and manifests included. A tool finds its config by
walking up from the files it governs, so a config placed beside them is exactly where the tool
looks:

- **`package.json`** — wherever it sits *defines a package*; move a sub-app's manifest beside its
  code and that folder becomes its own package.
- **`tsconfig.json`** — found by walking up from a source file; lives at or above the code it
  types.
- **`vite.config.ts`, `svelte.config.js`** — the build tool takes a `root`/`--config`; they
  belong with the app they build.
- **`Dockerfile`** — `docker build -f <path>` takes any path; lives with what it containerizes.

A file belongs at the repo root only when the root is what it governs — a harness's `.mcp.json`, an
app's own config file, whose discovery rule points literally at the project root.

## A cluster is a hidden boundary

Several files at one level all serving a single thing inside it — configs all for the web app,
assets all for one document, helpers all for one module — mean that thing wants to be its own
folder, with them gathered inside it, not splayed across the parent.

Pulling them in has a cost worth weighing: for code, the new folder becomes its own **package**,
with the monorepo ergonomics that brings. So it's a judgement call per case — but make the call.
The trap is reading the stranded cluster as the natural state of a root instead of a boundary
waiting to surface.

## Source and tests

`src/` and `test/` as root siblings is a deliberate answer to "where do tests live," not a bag:
tests are their own thing — a suite, run as a unit, often compiled apart — parented by the project
alongside the source they exercise, with `test/` mirroring `src/`'s shape so each test stays a
clear sibling of what it covers. Nesting tests *inside* the code they cover is the other sound
answer. What's wrong is neither — it's tests scattered with no consistent relationship to what they
test.
