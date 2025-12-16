---
title: Check if the Sentence Is Pangram
difficulty: 🟢 Easy
tags:
  - Hash Table
  - String
url: https://leetcode.com/problems/check-if-the-sentence-is-pangram/
---

# Check if the Sentence Is Pangram

## Problem Description

A **pangram** is a sentence where every letter of the English alphabet appears at least once.

Given a string `sentence` containing only lowercase English letters, return `true` if `sentence` is a **pangram**, or `false` otherwise.

## Examples

**Example 1:**

```plaintext
Input: sentence = "thequickbrownfoxjumpsoverthelazydog"
Output: true
Explanation: sentence contains at least one of every letter of the English alphabet.
```

**Example 2:**

```plaintext
Input: sentence = "leetcode"
Output: false
```

## Constraints

- `1 <= sentence.length <= 1000`
- `sentence` consists of lowercase English letters.

## Solution

The approach uses a hash map to track the occurrence of each letter in the alphabet. We initialize a dictionary with all 26 lowercase letters as keys, each mapped to a count of 0. Then we iterate through the input sentence, incrementing the count for each character encountered. Finally, we check if any letter has a count of 0 — if so, the sentence is not a pangram.

### Complexity Analysis

- **Time Complexity:** $O(n)$ - Single pass through the sentence plus a fixed 26-iteration check.
- **Space Complexity:** $O(1)$ - The alphabet dictionary has a fixed size of 26 entries.

```python
import string

class Solution:
    def checkIfPangram(self, sentence: str) -> bool:
        alphabet = {letter: 0 for letter in string.ascii_lowercase}

        for character in sentence:
            alphabet[character] += 1
        
        for letter in alphabet:
            if alphabet[letter] == 0:
                return False
        
        return True
```
