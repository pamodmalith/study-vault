---
type: concept
area: programming
date: 2026-11-05
tags: [example, algorithms]
status: done
---
# Binary search

## What is it?
A way to find a value in a **sorted** list by repeatedly halving the search range.

## Why does it exist?
Checking every item (linear search) is slow for big lists. Binary search needs only about log2(n) steps.

## How it works
- Look at the middle item.
- If it equals the target, done.
- If target is smaller, search the left half. Otherwise search the right half.
- Repeat until found or the range is empty.

## Small example
```python
def binary_search(items, target):
    lo, hi = 0, len(items) - 1
    while lo <= hi:
        mid = (lo + hi) // 2
        if items[mid] == target:
            return mid
        if items[mid] < target:
            lo = mid + 1
        else:
            hi = mid - 1
    return -1
```

## Common mistakes / gotchas
- The list must be sorted.
- Off-by-one errors with `lo` and `hi`.

## Source
- Lecture / own practice

## Related
- [[example-mean-median-mode]]

> This is an example note. Delete it when you start your own.
