# Pattern: Cyclic Sort

## Overview

**Cyclic Sort** is an in-place array technique for problems where values fall in a known range.

**Typical signals:**

- numbers are in the range `1..n` (or `0..n`)
- you need to find missing or duplicate values

> Core idea: while scanning index `i`, keep swapping `nums[i]` into its correct position until the current index is "resolved".

**Why does this work?**

Each swap moves at least one value into its final position (its home index). That means the number of misplaced values strictly decreases, so the process must finish in a linear number of swaps.

Once placement is done, the remaining mismatches directly tell you the answer:

- **missing**: an index `i` where the home value (`i + 1` for `1..n`, or `i` for `0..n`) never arrived
- **duplicate/corrupt**: a value sitting in a position that doesn't match its home

## Variations

### 1. Range `1..n` (index is `x - 1`)

**Use when:**

- Input values are in `1..n`

**Placement rule:**

- Value `x` belongs at index `x - 1`

### 2. Range `0..n` (index is `x`)

**Use when:**

- Input values are in `0..n` with length `n`

**Placement rule:**

- Value `x` belongs at index `x`, but skip value `n` (no index for it)

## Common Template

```python
i = 0
while i < n:
    correct_idx = f(nums[i])  # x-1 or x depending on the range

    if nums[i] is placeable and nums[i] != nums[correct_idx]:
        swap(nums[i], nums[correct_idx])
    else:
        i += 1
```

## Decision Guide

**Use when:**

- You see **values constrained to a range** like `1..n` or `0..n`
- The problem asks for:
  - missing numbers
  - duplicates
  - "one duplicated + one missing" (corrupt pair)
- You want an **in-place** solution with $O(1)$ extra space

## Pitfalls

- **Don't increment `i` after swapping**: you need to re-check the new value at `i`.
- **Watch bounds**: in `0..n` problems, ignore `n` (it's valid as a value but not as an index).

## Problems

|  #  | Problem                                                                                                  | Difficulty |
| :-: | :------------------------------------------------------------------------------------------------------- | :--------: |
| 01  | [Cyclic Sort](./01-cyclic-sort.md)                                                                       | 🟢 Easy    |
| 02  | [Missing Number](./02-find-the-missing-number.md)                                                        | 🟢 Easy    |
| 03  | [Find All Numbers Disappeared in an Array](./03-find-all-missing-numbers.md)                             | 🟢 Easy    |
| 04  | [Find the Duplicate Number](./04-find-the-duplicate-number.md)                                           | 🟡 Medium  |
| 05  | [Find All Duplicates in an Array](./05-find-all-duplicate-numbers.md)                                    | 🟡 Medium  |
| 06  | [Find the Corrupt Pair](./06-find-the-corrupt-pair.md)                                                   | 🟢 Easy    |
| 07  | [First Missing Positive](./07-find-the-first-missing-positive-number.md)                                 | 🔴 Hard    |
| 08  | [First K Missing Positive Numbers](./08-find%20the-first-K-missing-positive-numbers.md)                  | 🔴 Hard    |
