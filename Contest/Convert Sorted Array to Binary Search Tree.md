---
tags:
 - contest
 - easy
 - dfs
---

![[height_balanced_binary_search_tree.png]]

Дан список целых чисел, отсортированный по возрастанию, необходимо преобразовать его в двоичное дерево поиска, [[Деревья#^7c5a73|сбалансированное по высоте]].

## Решение



```Python
@dataclass
class TreeNode:
	val: int
	left: Optional[TreeNode]
	right: Optional[TreeNode]
```

```Python
def sorted_array_to_bst(self, nums: List[int]) -> Optional[TreeNode]:
	def dfs(arr):
		n = len(arr)
		if n < 1:
			return None
		i = n // 2
		node = TreeNode(arr[i])
		node.left = dfs(arr[:i])
		node.right = dfs(arr[i + 1:])
		return node
	return dfs(nums)
```

## Список источников

- [108. Convert Sorted Array to Binary Search Tree](https://leetcode.com/problems/convert-sorted-array-to-binary-search-tree/description/)