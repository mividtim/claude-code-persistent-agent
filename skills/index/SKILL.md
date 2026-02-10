---
description: Build or update the semantic index of the agent's memory vault. Scans for changed files and prints them for indexing.
allowed-tools: Bash, Read
argument-hint: "[scan|file <path>|update <path> <summary> <keywords>|stats]"
---

# Index Memory Vault

Build a keyword-based semantic index of all markdown files in `memory/`.
The index enables fast search across the vault without loading every file.

## Quick Start (full reindex)

1. Scan for files that need indexing:

```
Bash(command="python3 ${CLAUDE_PLUGIN_ROOT}/scripts/index-vault.py scan")
```

2. For each `NEEDS_INDEX` file, read it and generate a summary + keywords:

```
Bash(command="python3 ${CLAUDE_PLUGIN_ROOT}/scripts/index-vault.py file memory/path/to/note.md")
```

3. After reading the file, update the index with your summary and keywords:

```
Bash(command="python3 ${CLAUDE_PLUGIN_ROOT}/scripts/index-vault.py update memory/path/to/note.md 'One-line summary of the note' 'keyword1,keyword2,keyword3' 'related/file1.md,related/file2.md'")
```

4. Repeat for each file that needs indexing.

## Commands

- **scan** — Find files needing indexing. Compares content hashes to detect changes.
- **file `<path>`** — Print a file's content and hash for you to summarize.
- **update `<path>` `<summary>` `<keywords-csv>` `[related-csv]`** — Write an index entry.
- **stats** — Show index statistics (file counts, keyword counts, stale entries).

## Index Location

The index is stored at `memory/meta/semantic-index.json`. Each entry contains:
- `source_path` — path to the markdown file
- `content_hash` — short SHA-256 hash for change detection
- `summary` — one-line semantic summary (you generate this)
- `keywords` — list of searchable keywords (you generate these)
- `related` — optional list of related file paths

## Guidelines for Generating Keywords

- Include 5-15 keywords per file
- Mix concrete terms (names, tools, concepts) with abstract themes
- Include synonyms the searcher might use
- For identity/meta files, include the agent's name and role

If `$ARGUMENTS` is provided, pass it as the command (e.g., `/agency:index scan`).
