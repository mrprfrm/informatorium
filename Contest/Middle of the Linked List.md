---
authors:
  - Anton Petrov
status: note
tags:
  - easy
  - pointers
  - linkedlist
---

![Middle of the Linked List](middle_of_the_linked_list.png)

Given the head of a singly linked list, return the middle node of the linked list.

If there are two middle nodes, return the second middle node.

## Intuitive Solution

The most straightforward solution is to convert the linked list into an array and then return the middle element.

```python
class Solution:
    def middleNode(self, head: Optional[ListNode]) -> Optional[ListNode]:
        nodes = []
        while head is not None:
            nodes.append(head)
            head = head.next
        return nodes[len(nodes) // 2]
```

This solution is solid and simple. It has a time complexity of `O(n)` and a space complexity of `O(n)`, but it's still somewhat redundant. Instead of storing all nodes in a separate list, we can simply keep track of the middle node pointer. Let's dive into the approach.

## Pointer Solution

The main idea is to compute the current middle node as we iterate. On each step, we determine the number of nodes seen so far and divide it by 2 to estimate the current middle index.

```python
class Solution:
    def middleNode(self, head: Optional[ListNode]) -> Optional[ListNode]:
        mid_node = head
        prev_idx = i = 0
        while head is not None:
            mid_idx = (i + 1) // 2
            if mid_idx != prev_idx:
                mid_node = mid_node.next
                prev_idx = mid_idx
            head = head.next
            i += 1
        return mid_node
```

This solution maintains the same time complexity of `O(n)` but improves the space complexity to `O(1)`, resolving the redundancy issue. However, before we consider it the most efficient solution, let's walk through a concrete example.

## Fast/Slow Pointer Solution

Now, let's go through the linked list `[1, 2, 3, 4, 5, 6]` step by step:

```python
# Let's consider the following linked list:
linked_list = [1, 2, 3, 4, 5, 6]

# 1. i = 0, mid_idx = 0, mid_node = 1
linked_list = [1]
# 2. i = 1, mid_idx = 1, mid_node = 2
linked_list = [1, 2]
# 3. i = 2, mid_idx = 1, mid_node = 2
linked_list = [1, 2, 3]
# 4. i = 3, mid_idx = 2, mid_node = 3
linked_list = [1, 2, 3, 4]
# 5. i = 4, mid_idx = 2, mid_node = 3
linked_list = [1, 2, 3, 4, 5]
# 6. i = 5, mid_idx = 3, mid_node = 4
linked_list = [1, 2, 3, 4, 5, 6]
```

As we can see, there's a pattern: the middle node updates every second step. This suggests that we don't need to compute the middle index on every iteration—we can simply move the middle node pointer at half the speed of the head pointer.explore this idea with an example:

```python
linked_list = [1, 2, 3, 4, 5, 6]
# i is the slow pointer, j is the fast pointer
i = j = 0

# 1. i = 1, j = 2
linked_list = [1, 2, 3]
# 2. i = 2, j = 4
linked_list = [1, 2, 3, 4, 5]
# 3. i = 3, j = 6
linked_list = [1, 2, 3, 4, 5, 6]
```

As shown above, this approach reaches the middle node in roughly half the number of steps. Of course, we can't literally skip steps for the fast pointer since we don’t know the list's length, but we can traverse using the child’s child node (`head.next.next`):

```python
class Solution:
    def middleNode(self, head: Optional[ListNode]) -> Optional[ListNode]:
        mid_node = head
        while head is not None and head.next is not None:
            mid_node = mid_node.next
            head = head.next.next
        return mid_node
```

In fact, the final version implements the fast/slow pointer approach. It has the same `O(n)` time and `O(1)` space complexity, but requires roughly half as many loop iterations as the previous methods.

## References

- [876. Middle of the Linked List](https://leetcode.com/problems/middle-of-the-linked-list)
