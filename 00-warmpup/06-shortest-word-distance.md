---
title: Shortest Word Distance
difficulty: 🟢 Easy
tags:
  - Array
  - String
url: https://leetcode.com/problems/shortest-word-distance/
---

# Shortest Word Distance

## Problem Description

Given an array of strings `wordsDict` and two different strings that already exist in the array `word1` and `word2`, return the shortest distance between these two words in the list.

## Examples

**Example 1:**

```plaintext
Input: wordsDict = ["practice", "makes", "perfect", "coding", "makes"], word1 = "coding", word2 = "practice"
Output: 3
```

**Example 2:**

```plaintext
Input: wordsDict = ["practice", "makes", "perfect", "coding", "makes"], word1 = "makes", word2 = "coding"
Output: 1
```

## Constraints

- `2 <= wordsDict.length <= 3 * 10^4`
- `1 <= wordsDict[i].length <= 10`
- `wordsDict[i]` consists of lowercase English letters.
- `word1` and `word2` are in `wordsDict`.
- `word1 != word2`

## Solution

The approach uses two pointers to track the most recent positions of `word1` and `word2` while traversing the array. Initialize both positions to `-1`. As we iterate, update the corresponding position when we encounter either word. Whenever both positions are valid, compute the absolute difference and update the minimum distance.

### Complexity Analysis

- **Time Complexity:** $O(n)$ - Single pass through the array.
- **Space Complexity:** $O(1)$ - Only constant extra space for tracking positions.

```python
class Solution:
    def shortestDistance(self, words: List[str], word1: str, word2: str) -> int:
        word1_index = -1
        word2_index = -1
        distance = float("inf")

        for i in range(len(words)):
            if words[i] == word1:
                word1_index = i

            if words[i] == word2:
                word2_index = i

            if word1_index != -1 and word2_index != -1:
                distance = min(distance, abs(word1_index - word2_index))

        return distance
```
