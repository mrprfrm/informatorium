---
authors:
  - Anton Petrov
status: draft
tags:
  - medium
  - pointers
  - prefixsum
---

Given an integer array `nums`, return an array `answer` such that `answer[i]` is equal to the product of all the elements of `nums` except `nums[i]`.

The product of any prefix or suffix of `nums` is **guaranteed** to fit in a **32-bit** integer.

You must write an algorithm that runs in `O(n)` time and without using the division operation.

## Intuitive Solution

According to the problem statement, we are not allowed to use an embedded loop over the `nums` array nor the division operation. To be completely honest, none of the restrictions are actually limiting our implementation. An embedded loop is obviously not an efficient way to solve the problem, and division is not a viable solution either — for instance, we can have a zero in the array, and the total product will be zero as well. So we need to come up with a different approach. Let's consider a specific example:

```python
nums = [1, 2, 3, 0 , 5, 6 , 7]
total_product = 0
answer = [0, 0, 0, 0, 0, 0, 0]
```

Instead, we can try to calculate the cumulative product across the array. For more convenient index management, let's use a prefix sum–like implementation:

```python
nums = [1, 2, 3, 0 , 5, 6 , 7]
product = [1] * (len(nums) + 1)
for i, num in enumerate(nums):
    product[i + 1] = product[i] * num
product # [1, 1, 2, 6, 0, 0, 0, 0]
```

Now we have a cumulative product, but it seems pointless since after the first zero in the original list, any further products have no effect. But before we move in a different direction, let's take a closer look at the product before zero was encountered:

```python
nums = [1, 2, 3, 0 , 5, 6 , 7]
product = [1, 1, 2, 6, 0, 0, 0, 0]

# Here the index of zero is 3 for prefix products
# associated with this element we also need to use index 3
product[3] # 6
```

Now, let's consider what other operations we need to perform to reach a proper result. We need to have the same prefix product but from the end of the array, which means we need a suffix product. Let's look into it:

```python
nums = [1, 2, 3, 0 , 5, 6 , 7]
product = [1] * (len(nums) + 1)
for i in reversed(range(n)):
    product[i - 1] = product[i] * nums[i]
product # [0, 0, 0, 0, 210, 42, 7, 1]
```

Now let's combine both — let's find the product for the zero element based on both prefix products:

```python
nums = [1, 2, 3, 0 , 5, 6 , 7]
prefix_product = [1, 1, 2, 6, 0, 0, 0, 0]
suffix_product = [0, 0, 0, 0, 210, 42, 7, 1]

# To find the product for the zero element, we need to multiply prefix and suffix products
# with the same index after aligning both prefix and suffix arrays
answ[3] = product[3] * product[-4] # 6 * 210 = 1260

# To make index management even more convenient, we can slice both prefix and suffix products
# from the right and left respectively and use only one index instead of redundant calculations

prefix_product = prefix_product[:n]
suffix_product = suffix_product[1:]
answ[3] = prefix_product[3] * suffix_product[3] # 6 * 210 = 1260
```

Now let's consider the entire implementation:

```python
class Solution:
    def productExceptSelf(self, nums: List[int]) -> List[int]:
        n = len(nums)
        prefix_prod = [1] * (n + 1)
        suffix_prod = prefix_prod.copy()

        for i, num in enumerate(nums):
            prefix_prod[i + 1] = prefix_prod[i] * num

        for i in reversed(range(n)):
            suffix_prod[i - 1] = suffix_prod[i] * nums[i]

        prefix_prod, suffix_prod = prefix_prod[:n], suffix_prod[1:]

        answ = []
        for i, num in enumerate(prefix_prod):
            answ.append(num * suffix_prod[i])
        return answ
```

> Instead of slicing the product arrays, we can simply use `range(1, n)` and appropriate indexes in the loop. For this particular case, this approach has an efficiency impact since it allows us to avoid redundant slicing.

This solution has solid `O(n)` time and `O(n)` space complexity, but if we take a closer look at it, we can see that within those bounds, there's still at least one redundant loop and an extra list. Let's see if we can do better.

## Optimal Solution

The main idea here is to process both the suffix product and the final answer calculation in the same loop. Let's consider an example of how we can reach this optimization:

```python
nums = [1, 2, 3, 0 , 5, 6 , 7]
prefix_product = [1, 1, 2, 6, 0, 0, 0]

# Now it's time to calculate the suffix, but according to the previous implementation,
# it seems like we don't actually need to store suffix products

# Instead, we can simply loop through the prefix product array in reverse order,
# and for each element from the right, we can find an appropriate suffix product

suffix_prod = 1
suffix_prod *= nums[-1]
answ[-1] = prefix_product[-1] * suffix_prod # 0

# And so on...

suffix_prod *= nums[-3]
answ[-3] = prefix_product[-3] * suffix_prod # 6 * 210 = 1260
```

Now let's proceed with the implementation:

```python
class Solution:
    def productExceptSelf(self, nums: List[int]) -> List[int]:
        n = len(nums)
        prefix_prod = [1] * n

        for i in range(1, n):
            prefix_prod[i] = prefix_prod[i - 1] * nums[i - 1]

        answ = [1] * n
        suffix_prod = 1
        for i in reversed(range(n)):
            answ[i] = prefix_prod[i] * suffix_prod
            suffix_prod *= nums[i]

        return answ
```

Currently, the implementation has the same `O(n)` time and space complexity, but it is obviously much more optimized since we don't have any redundant loops or lists, which may be crucial for large input data.

## References

- [238. Product of Array Except Self](https://leetcode.com/problems/product-of-array-except-self/description/)

