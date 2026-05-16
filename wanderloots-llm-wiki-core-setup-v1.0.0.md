# LLM Wiki Core Setup Guide For Agents

> Give this file to an AI coding agent when you want it to build a basic LLM Wiki from an empty Obsidian vault or empty git repo. This is the free setup guide. It is not a prebuilt kit, which includes the exact tested starter files plus the advanced modules in the How LLM Wiki video: 
> More context on Why LLM Wiki: 
## Mission

Build a clean, beginner-friendly LLM Wiki in Obsidian.

The finished core system should let the user put source notes in `Raw/Sources/`. The agent or LLM connection can then:
1. Compile those sources into concise, reusable notes in `Wiki/`.
2. Keep claims traceable from Wiki notes back to Raw sources.
3. Build indexes and a machine-readable catalog.
4. Run deterministic health checks before committing changes.
5. Ask future agents to query the compiled Wiki before reading broad Raw context.

## What To Ask The Agent

Use this prompt when starting from empty:

```text
Use llms-core-setup.md to build the core LLM Wiki in this folder. Follow the setup order exactly, commit after each major step if git is available, run the maintenance checks, and stop once tutorial-06-query-and-lint passes.
```

Use this prompt after the core system exists:

```text
Read AGENTS.md and inspect the current LLM Wiki. Search the catalog before opening Raw sources. Compile any unprocessed Raw sources into concise Wiki notes, keep every claim linked to sources, rebuild indexes, run lint/source checks, and summarize what changed.
```

## What This File Can Rebuild

This file is sufficient for a capable AI coding agent to build a functional core LLM Wiki from an empty folder, including:
- Raw source layer
- compiled Wiki layer
- Schema/rules layer
- note templates
- core agent skills
- deterministic `wiki_tool.py`
- source manifest
- catalog and indexes
- lint and audit gates

## Before Editing

Inspect the target folder first.

If it is not a git repo, initialize git:

```bash
git init
```

If it is already an Obsidian vault, preserve existing `.obsidian/` basics and ignore plugin state, caches, workspace churn, private files, and binary source files by default.

Create a safe `.gitignore` that ignores:

```gitignore
.DS_Store
.obsidian/workspace*.json
.obsidian/plugins/
.obsidian/cache/
.obsidian/logs/
Raw/Files/*
!Raw/Files/.gitkeep
Drafts/
```

## Core Build Order

Follow this order. Do not skip ahead.
### Step 00: Empty Vault

Create or preserve:

- `.gitignore`
- `Welcome.md`

Commit message:

```text
tutorial-00-empty-vault
```

### Step 01: Core Structure

Create these folders:

```text
Raw/Sources/
Raw/Files/
Wiki/Topics/
Wiki/Concepts/
Wiki/Entities/
Wiki/Projects/
Wiki/Logs/
Schema/
_templates/
.agents/skills/
scripts/
tutorial/
```

Add `.gitkeep` files only where needed to preserve empty folders.

Commit message:

```text
tutorial-01-core-structure
```

### Step 02: Schema And Agent Rules

Create:
- `AGENTS.md`
- `Schema/frontmatter-schema.md`
- `Schema/workflow-examples.md`
- `Schema/lint-checklist.md`
- `Schema/naming-conventions.md`
- `.agents/skills/llm-wiki-ingest/SKILL.md`
- `.agents/skills/llm-wiki-query/SKILL.md`
- `.agents/skills/llm-wiki-lint/SKILL.md`
- `.agents/skills/llm-wiki-maintain/SKILL.md`

`AGENTS.md` must tell agents:
- Treat `Raw/Sources/` as source material, not as compiled notes.
- Write reusable knowledge only under `Wiki/`.
- Keep every compiled note linked to one or more Raw sources.
- Search `Wiki/catalog.jsonl` before opening broad Raw context.
- Run `build`, `lint`, and source checks before commits.
- Do not invent citations or create unsupported claims.

Commit message:

```text
tutorial-02-schema-and-agents
```

### Step 03: Templates

Create:

- `_templates/source-note.md`
- `_templates/concept-note.md`
- `_templates/topic-note.md`
- `_templates/entity-note.md`
- `_templates/project-note.md`
- `_templates/log-note.md`

Source notes should include:

```yaml
---
Title: ""
Author: ""
Reference: ""
ContentType:
  - "markdown"
Created: YYYY-MM-DD
Processed: false
tags:
  - "source"
---
```

Compiled Wiki notes should include:

```yaml
---
tags:
  - "concept"
topics: []
status: seed
created: YYYY-MM-DD
updated: YYYY-MM-DD
sources: []
source_count: 0
aliases: []
---
```

Allowed compiled note tags:

- `topic`
- `concept`
- `entity`
- `project`
- `log`

Commit message:

```text
tutorial-03-templates
```

### Step 04: Deterministic Tooling

Create `scripts/wiki_tool.py` using only the Python standard library.

Required commands:

- `doctor`: non-mutating health check for folders, Python version, catalog, source manifest, and basic note counts.
- `build`: generate `Wiki/catalog.jsonl`, `Wiki/index.md`, and per-folder index files.
- `lint`: validate compiled Wiki note frontmatter, allowed tags, source links, and `source_count`.
- `source-scan`: list Raw sources and optionally update `Schema/source-manifest.jsonl`.
- `source-scan --update --accept-covered`: update the source manifest after Wiki notes cover Raw sources.
- `source-lint`: validate source frontmatter and source coverage state.
- `source-delta`: show Raw sources not represented in the manifest.
- `source-coverage`: show which Raw sources are covered by compiled Wiki notes.
- `search-catalog --query "text"`: search compiled Wiki notes through the catalog.
- `log --title "title" --details "details"`: append a short entry to `Wiki/log.md`.

Create:

- `scripts/install_hooks.sh`
- `.githooks/pre-commit`
- `scripts/audit_public.py`
- `Schema/command-reference.md`

The pre-commit hook may run:

```bash
python3 scripts/wiki_tool.py build
python3 scripts/wiki_tool.py lint
python3 scripts/wiki_tool.py source-lint
```

`audit_public.py` should fail on obvious secrets, machine-local paths, private keys, and plugin/cache state.

Minimum catalog contract:

`Wiki/catalog.jsonl` should contain one JSON object per compiled Wiki note. Each object should include:

```json
{"path":"Wiki/Concepts/example.md","title":"Example","tag":"concept","topics":[],"sources":[],"updated":"YYYY-MM-DD"}
```

Minimum source manifest contract:

`Schema/source-manifest.jsonl` should contain one JSON object per Raw source. Each object should include:

```json
{"path":"Raw/Sources/example.md","title":"Example","processed":true,"covered_by":["Wiki/Concepts/example.md"],"updated":"YYYY-MM-DD"}
```

Minimum lint behavior:

- compiled Wiki notes must use one allowed tag: `topic`, `concept`, `entity`, `project`, or `log`
- compiled Wiki notes must keep `source_count` equal to the number of `sources`
- compiled Wiki note source links should point to existing files under `Raw/Sources/`
- Raw source notes should include `Title`, `Reference`, `Created`, `Processed`, and `tags`
- `source-lint` should fail if a source is marked processed but has no Wiki coverage

Commit message:

```text
tutorial-04-tooling
```

### Step 05: First Ingest

Add one small, creator-owned source in `Raw/Sources/`. For the tutorial, the intended source is the creator's `Why LLM Wiki` video source.

If the user has not supplied a source, ask them for one. If they explicitly want a fully self-contained demo, create this tiny fixture source:

```markdown
---
Title: "LLM Wiki Starter Demo Source"
Author: "LLM Wiki Starter"
Reference: "owned-demo"
ContentType:
  - "markdown"
Created: YYYY-MM-DD
Processed: false
tags:
  - "source"
---

# LLM Wiki Starter Demo Source

An LLM Wiki separates captured source material from compiled knowledge notes. Raw sources preserve original context, while Wiki notes turn useful claims into short, linked, reusable knowledge. The most useful workflow is to search the compiled Wiki first, then open Raw sources only when more evidence or detail is needed.
```

Then compile it into a few Wiki notes, for example:

- one topic note
- one concept note
- one project note
- one log note

Every compiled Wiki note must link back to the Raw source in `sources`.

Do not rely on generated text alone. Keep the transformation visible: a small Raw source should become focused Wiki notes.

Commit message:

```text
tutorial-05-first-ingest
```

### Step 06: Query And Lint

Run:

```bash
python3 scripts/wiki_tool.py doctor
python3 scripts/wiki_tool.py build
python3 scripts/wiki_tool.py lint
python3 scripts/wiki_tool.py source-scan --update --accept-covered
python3 scripts/wiki_tool.py source-lint
python3 scripts/wiki_tool.py search-catalog --query "llm wiki"
python3 scripts/audit_public.py
```

Commit generated core artifacts:

- `Schema/source-manifest.jsonl`
- `Wiki/catalog.jsonl`
- `Wiki/index.md`
- per-folder `index.md` files

Commit message:

```text
tutorial-06-query-and-lint
```

At this point the user has a working core LLM Wiki.

## Final Acceptance Criteria

The core build is complete only when all of these are true:

- `AGENTS.md` exists and describes Raw/Wiki/Schema rules
- `_templates/` contains source, concept, topic, entity, project, and log templates
- `.agents/skills/` contains ingest, query, lint, and maintain skills
- `scripts/wiki_tool.py` supports every command listed in Step 04
- `scripts/audit_public.py` exists and passes
- `Wiki/catalog.jsonl` exists and includes the compiled Wiki notes
- `Schema/source-manifest.jsonl` exists and covers processed Raw sources
- `Wiki/index.md` and per-folder indexes exist
- at least one Raw source exists
- at least one compiled Wiki note links back to that Raw source
- no advanced `bonus/` folder exists unless the user explicitly asked for it
- the maintenance gate passes

## Ingest Workflow For Future Sources

When the user adds a new source:

1. Put cleaned Markdown in `Raw/Sources/`.
2. Run `search-catalog` for likely related topics.
3. Open only the most relevant compiled Wiki notes.
4. Create or update focused notes in `Wiki/`.
5. Add Raw source links to `sources`.
6. Keep `source_count` accurate.
7. Run:

```bash
python3 scripts/wiki_tool.py build
python3 scripts/wiki_tool.py lint
python3 scripts/wiki_tool.py source-scan --update --accept-covered
python3 scripts/wiki_tool.py source-lint
```

8. Add a log entry if the ingest meaningfully changed the Wiki.

## Query Workflow

When answering a question from the Wiki:

1. Start with `Wiki/index.md`.
2. Search the catalog:

```bash
python3 scripts/wiki_tool.py search-catalog --query "user topic"
```

3. Open the most relevant Wiki notes.
4. Open Raw sources only when the compiled note is insufficient or the user asks for source-level verification.
5. Cite the compiled note and Raw source when the answer depends on source material.

## Maintenance Gate

Before every meaningful commit, run:

```bash
python3 scripts/wiki_tool.py doctor
python3 scripts/wiki_tool.py build
python3 scripts/wiki_tool.py lint
python3 scripts/wiki_tool.py source-lint
python3 scripts/audit_public.py
```

After source ingestion, also run:

```bash
python3 scripts/wiki_tool.py source-scan --update --accept-covered
python3 scripts/wiki_tool.py source-lint
```
