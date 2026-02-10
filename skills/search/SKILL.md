---
description: Search the agent's memory vault by keyword. Returns ranked results with scores, summaries, and matched keywords.
allowed-tools: Bash, Read
argument-hint: "<query>"
---

# Search Memory Vault

Search the semantic index built by `/agency:index`. Finds memory files by
keyword overlap between your query and each file's indexed keywords and summary.

## Search (human-readable output)

```
Bash(command="python3 ${CLAUDE_PLUGIN_ROOT}/scripts/index-vault.py search '$ARGUMENTS'")
```

Results are ranked by relevance score:
- Full keyword match = 1.0 points
- Summary term match = 0.5 points
- Top 10 results shown

## Search with JSON output (for reranking)

```
Bash(command="python3 ${CLAUDE_PLUGIN_ROOT}/scripts/index-vault.py search-json '$ARGUMENTS'")
```

Returns structured JSON with candidates, scores, and matched keywords.
Use this when you want to rerank results by reading summaries and
selecting the most relevant files to deep-read.

## Logging Misses

If a search didn't surface a file you expected, log it for evaluation:

```
Bash(command="python3 ${CLAUDE_PLUGIN_ROOT}/scripts/index-vault.py miss 'query terms' 'expected/file/path.md' 'optional reason'")
```

Review the miss log to improve keywords:

```
Bash(command="python3 ${CLAUDE_PLUGIN_ROOT}/scripts/index-vault.py misses")
```

## Tips

- Use short, specific queries: `identity drift` not `what is identity drift`
- If no results, the index may need updating — run `/agency:index scan`
- After finding candidates via search, deep-read the top results
- Log misses — they help you improve keyword coverage over time

`$ARGUMENTS` is passed as the search query.
