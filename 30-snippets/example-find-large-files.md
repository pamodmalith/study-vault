---
type: snippet
language: bash
date: 2026-11-05
tags: [example, linux]
---
# Find the largest files in a folder

## What it does
Lists the 10 biggest files under the current directory.

## Snippet
```bash
find . -type f -exec du -h {} + | sort -rh | head -n 10
```

## Notes
- `sort -rh` sorts human-readable sizes, largest first.

> This is an example note. Delete it when you start your own.
