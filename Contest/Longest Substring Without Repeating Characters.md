---
authors:
  - Anton Petrov
status: draft
tags:
  - medium
  - hashmap
  - window
---

Given a string `s`, find the length of the longest substring[^1] without duplicate characters.

## Intuitive Solution

According to the definition of a substring[^1], we can move through the original string and build the current substring by adding new unique characters until we encounter a duplicate. Once we hit a duplicate, we need to shrink the substring from the left until no duplicates remain.

```python
from collections import defaultdict

class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        if len(s) < 1:
            return 0

        i = 0
        chars = defaultdict(int)
        max_len = 1
        for j, char in enumerate(s):
            chars[char] += 1

            while i < j and chars[char] > 1:
                c = s[i]
                chars[c] -= 1
                i += 1

            max_len = max(j - i + 1, max_len)

        return max_len
```

This solution has `O(n)` time and `O(n)` space complexity. We use a single loop to iterate through the string and a single hashmap to store the characters we've encountered.

## References

- [3. Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/description/)

[^1]: A **substring** is a contiguous, non-empty sequence of characters within a string.
