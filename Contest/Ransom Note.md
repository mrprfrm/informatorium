---
authors:
  - Anton Petrov
status: note
tags:
  - easy
---

Given two strings `ransomNote` and `magazine`, return `true` if `ransomNote` can be constructed by using the letters from `magazine` and `false` otherwise.

Each letter in `magazine` can only be used once in `ransomNote`.

## Intuitive Solution

The most obvious solution is to convert the `magazine` string into a counter that tracks the occurrences of each character. Then, as we iterate through `ransomNote`, we decrement the counter for each character. If at any point a character is missing or its count drops below one, we can conclude that `ransomNote` cannot be constructed.

```python
class Solution:
    def canConstruct(self, ransomNote: str, magazine: str) -> bool:
        # Convert the magazine string into a counter
        memo = defaultdict(int)
        for char in magazine:
            memo[char] += 1

        for char in ransomNote:
            if char not in memo or memo[char] < 1:
                return False

            memo[char] -= 1
        return True
```

The time complexity of this solution is `O(n)`, which is quite efficient. However, instead of using an intermediate counter, we can leverage Python's string API directly.

## Optimized Solution

The most significant improvement we can make is eliminating the unnecessary counter, which helps reduce space usage. Additionally, we don't need to iterate through the entire `ransomNote`; we only need to check if each unique character appears at least as many times in `magazine`.

```python
class Solution:
    def canConstruct(self, ransomNote: str, magazine: str) -> bool:
        for char in set(ransomNote):
            if ransomNote.count(char) > magazine.count(char):
                return False
        return True
```

Moreover, according to the problem statement, we only need to verify that all characters in `ransomNote` are present in `magazine`. Instead of using a loop, we can replace it with the `all` function, making our solution even more concise and efficient.

```python
class Solution:
    def canConstruct(self, ransomNote: str, magazine: str) -> bool:
        return all(ransomNote.count(char) <= magazine.count(char) for char in set(ransomNote))
```

