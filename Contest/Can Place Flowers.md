---
authors:
  - Anton Petrov
status: note
tags:
  - easy
  - pointers
  - window
---

You have a long flowerbed in which some of the plots are planted, and some are not. However, flowers cannot be planted in **adjacent** plots.

Given an integer array flowerbed containing `0`'s and `1`'s, where `0` means empty and `1` means not empty, and an integer `n`, return `true` if n new flowers can be planted in the flowerbed without violating the no-adjacent-flowers rule and false otherwise.

## Intuitive Solution

The condition for planting a new flower is that the current position is empty and both the previous and next positions are empty as well.

If the current empty position is at the beginning or end of the flowerbed, we only need to check one adjacent position. However, instead of handling these cases separately, we can set default values for the first and last positions to be `0`, allowing us to check all positions in the same way.

```python
class Solution:
    def canPlaceFlowers(flowerbed: List[int], n: int) -> bool:
        count = 0
        m = len(flowerbed)
        for i in range(m):
            is_left_empty, is_right_empty = False, False
            if flowerbed[i] == 0:
                is_left_empty = i == 0 or flowerbed[i - 1] == 0
                is_right_empty = i == m - 1 or flowerbed[i + 1] == 0

            if is_left_empty and is_right_empty:
                flowerbed[i] = 1
                count += 1

            if count >= n:
                return True
        return False
```

## Sliding Window Solution

In fact, we are looking for subarrays with `k` empty positions (for this particular problem `k = 3`). Instead of directly checking the neighbor positions, we can set a window of `size <= k` and when we encounter a flower or reach the `k` size, we can increment the number of available positions for planting a new flower.

As we decided to use the sliding window technique, we need to consider the edge cases when the window is at the beginning or end of the flowerbed. To keep the logic common, we can add two extra empty positions at the beginning and end of the flowerbed.

```python
class Solution:
    def canPlaceFlowers(self, flowerbed: List[int], n: int) -> bool:
        memo = defaultdict(int)
        i = 0
        res = 0
        # Add extra positions at the beginning and end of the flowerbed
        flowerbed = [0] + flowerbed + [0]
        for j in range(1, len(flowerbed)):
            num = flowerbed[j]
            memo[num] += 1

            # Max window size is 3, so we remove redundant elements from its left side
            if j - i + 1 > 3:
                m = flowerbed[i]
                memo[m] -= 1
                i += 1

            # In case the window size is sufficient and all its elements are empty
            if j - i + 1 >= 3 and memo[1] == 0:
                res += 1

                # Plant a new flower in the middle of the window
                flowerbed[i + 1] = 1
                memo[1] += 1

                # Decrease the size of the window to make it available for new elements
                m = flowerbed[i]
                memo[m] -= 1
                i += 1

            if res >= n:
                return True
        return False
```

## Optimized Solution

Now, let's try counting the number of flowers we can plant in each gap between already planted flowers in the flowerbed. Let's consider an example:

```python
# Example 1
n, flowerbed = 1, [1, 0, 0, 0, 1]

# The gap between the first and second flowers is 3
gap = [0, 0, 0]

len(gap) // 2 >= n  # returns True
```

Now we can determine how many new flowers we can plant in each gap, but there is at least one edge case to consider:

```python
# Example 2
n, flowerbed = 3, [0, 0, 0, 0, 0]

# The gap is the entire flowerbed
gap = [0, 0, 0, 0, 0]

len(gap) // 2 >= n  # returns False
```

The answer is wrong because we can plant 3 flowers in the flowerbed like this: `[1, 0, 1, 0, 1]`. Instead, the current logic is counting flowers that can be planted between `0` and `m - 1`, like this: `[0, 1, 0, 1, 0]`.

To make the solution work correctly, let's assume we have one extra empty position at the beginning and start counting from `1` instead of `0`. According to this, the total number of flowers we can plant in the gap is:

$$
len(gap) + 1) // 2
$$

> It is important to note that this calculation only works if the gap starts at the beginning of the flowerbed and ends at the end of it.

```python
# Example 2 (Adjusted)
n, flowerbed = 3, [0, 0, 0, 0, 0]

# The gap is the entire flowerbed
gap = [0, 0, 0, 0, 0]

(len(gap) + 1) // 2 >= n  # returns True
```

Now we need to cover one more edge case when there are multiple gaps in the flowerbed. As mentioned earlier, we can't use the same logic as for an empty flowerbed. Instead, we need to calculate the number of flowers we can plant in each gap separately.

```python
n, flowerbed = 1, [0, 1, 0, 0, 0]

# The gaps in the flowerbed:
gap1 = [0]
gap2 = [0, 0, 0]

res = len(gap1) // 2  # Set res to 0
res += len(gap2) // 2  # Set res to 1
res >= n  # returns True
```

Concluding, we have two cases where the calculation of flowers in gaps is different:

- The entire flowerbed is empty: We need to add an extra empty position at the beginning.
- The flowerbed has one or more gaps between planted flowers: We count the number of flowers that can be planted in each gap separately, without any extra space.

Now let's implement the optimized solution, but instead of counting gaps' lengths, we will count flowers on each iteration and update the counter each time we encounter a flower.

```python
class Solution:
    def canPlaceFlowers(self, flowerbed: List[int], n: int) -> bool:
        res = 0
        # We start with an extra space at the beginning
        count = 1
        for num in flowerbed:
            if num == 0:
                count += 1
            else:
                # When we encounter a flower, we need to remove the extra space
                # we added at the beginning
                count = count - 1 if count > 0 else 0
                res += count // 2
                count = 0

            if res >= n:
                return True

        # The gap might end at the end of the flowerbed,
        # so we need to recalculate the last count value
        # since the loop didn’t have enough iterations to do so.
        return res + count // 2 >= n
```

## References

- [605. Can Place Flowers (leetcode.com)](https://leetcode.com/problems/can-place-flowers/description/?envType=problem-list-v2&envId=m424e3ds)
