---
authors:
  - Anton Petrov
status: draft
tags:
  - prefixsum
---

The idea of the Prefix Sum algorithm is simple: we can perform cumulative calculations on the array elements and store the results in an appropriate data structure. This allows us to efficiently process range queries on subarrays in constant time.

Let's consider an example: we have an array of integers `nums` and an integer `k`. Let's find the maximum sum of a subarray of size `k`.

```python
def get_max_sum(nums: List[int], k: int) -> int:
    # The prefix sum array
    prefix_sum = [0] * (len(nums) + 1)
    for i, num in enumerate(nums):
        prefix_sum[i + 1] = prefix_sum[i] + num

    max_sum = -float("inf")
    # The sum between the i-th and (i + k)-th elements
    # is equal to the difference between the (i - k)-th element of the prefix sum array
    # and the i-th element of the prefix sum array
    for i in range(k, len(nums) + 1):
        max_sum = max(prefix_sum[i] - prefix_sum[i - k], max_sum)
    return max_sum
```

The `i`-th element of the prefix sum represents the sum of all elements from the beginning of the original array up to the `i - 1`-th element. For example:

```python
# j  =  0  1  2  3  4
nums = [1, 2, 3, 4, 5]
# i  =  0  1  2  3   4   5
pref = [0, 1, 3, 6, 10, 15]

# The sum of the subarray [2, 3, 4] is equal to the difference between the 4th and 1st indexes of the prefix sum array
summ = pref[4] - pref[1] # 10 - 1 = 9
```

Note that the prefix sum does not necessarily have to store only cumulative sums of the array elements. It can also store counts, products, or results of other operations. Let's take a closer look at a related problem.

## Cumulative Count

Let's define the problem statement: we have an array `nums` consisting of only ones and zeros, along with two integers `l` and `r` that represent the left and right boundaries of a subarray. Our task is to find the number of zeros in the subarray between `l` and `r`.

```python
def get_zeros_count(nums: List[int], l: int, r: int) -> int:
    # The prefix sum array
    prefix_sum = [0] * (len(nums) + 1)
    for i, num in enumerate(nums):
        if num == 0:
            prefix_sum[i + 1] = prefix_sum[i] + 1

    return prefix_sum[r] - prefix_sum[l]
```

Now, let's consider a specific example:

```python
# j  =  0  1  2  3  4  5  6  7  8
nums = [1, 0, 1, 1, 0, 1, 0, 0, 1]
# i  =  0  1  2  3  4  5  6  7  8  9
pref = [0, 0, 1, 1, 1, 2, 2, 3, 4, 4]

# The number of zeros in the subarray [1, 0, 1, 0, 0] is equal to the difference between the 8th and 3rd indexes of the prefix sum array
count = pref[8] - pref[3] # 4 - 1 = 3
```

As mentioned earlier, the Prefix Sum concept allows for flexibility in intermediate storage structures. Let's explore a more advanced application.

## Advanced Prefix Sum

Consider the following problem: given an array of integers `nums`, we need to find the number of subarrays whose elements sum to zero.

Let's begin by computing the prefix sum and identifying patterns that can help us derive the final solution.

```python
# j  =  0  1   2  3   4  5   6  7
nums = [1, 2, -3, 4, -2, 1, -3, 0]

# i  =  0  1  2  3  4  5  6  7  8
pref = [0, 1, 3, 0, 4, 2, 3, 0, 0]
# zeros |        |           |  |

# As we can see, there are multiple zero values in the prefix sum,
# indicating that the sum of the subarray between these positions is zero
sum([1, 2, -3]) == 0 # returns True
sum([1, 2, -3, 4, -2, 1, -3]) == 0 # returns True
sum([1, 2, -3, 4, -2, 1, -3, 0]) == 0 # returns True

sum([4, -2, 1, -3]) == 0 # returns True
sum([4, -2, 1, -3, 0]) == 0 # returns True

sum([0]) == 0 # returns True
```

A prefix sum of zero for a particular subarray means that the sum of its elements did not alter the default value of zero.

From this observation, we can conclude that not only subarrays whose prefix sum is zero are valid, but also those whose prefix sum has remained unchanged since a previous occurrence.

```python
nums = [1, 2, -3, 4, -2, 1, -3, 0]
# j  =  0  1   2  3   4  5   6  7

# i  =  0  1  2  3  4  5  6  7  8
pref = [0, 1, 3, 0, 4, 2, 3, 0, 0]
# same  |     |  |        |  |  |

sum([-3, 4, -2, 1]) == 0 # returns True
```

Now let's formulate the pattern: for each `n` subarrays with the same prefix sum, the total number of valid subarrays can be computed using the formula:

$$
s = \frac{n \times (n - 1)}{2}
$$

where:

- `n` is the number of times a particular prefix sum appears,
- `s` is the total number of valid subarrays that can be formed.

This formula follows the arithmetic series principle where the common difference is `1`.

Let's implement the final solution:

```python
def get_zero_sum_subarrays(nums: List[int]) -> int:
    # The prefix sum array
    prefix_sum = [0] * (len(nums) + 1)
    for i, num in enumerate(nums):
        prefix_sum[i + 1] = prefix_sum[i] + num

    # The counter for prefix sum values
    count = defaultdict(int)
    for num in prefix_sum:
        count[num] += 1

    res = 0
    for summ, n in count.items():
        res += n * (n - 1) // 2

    return res
```

Now that we understand the core concept, let's optimize the solution:

```python
def get_zero_sum_subarrays(nums: List[int]) -> int:
    # We don't need the prefix sum array, only the last prefix sum value,
    # which we can store in the counter.
    summ = 0
    count = defaultdict(int)
    count[0] = 1
    for num in nums:
        summ += num
        count[summ] += 1

    res = 0
    for n in count.values():
        res += n * (n - 1) // 2
    return res
```

> The final solution is no longer a pure prefix sum implementation, but the prefix sum concept was key to understanding the problem and deriving an optimal solution.

## Conclusion

The Prefix Sum concept has many variations and can be adapted to different problems. It is not limited to zero-sum subarrays—we can also find subarrays with a sum equal to `k` or compute other operations like products.

A key requirement for its efficiency is that the original array remains immutable. If the array is modified, previously computed prefix sums become outdated, requiring recalculations for correctness. This negates the benefit of constant-time range queries and leads to linear time complexity for updates.
