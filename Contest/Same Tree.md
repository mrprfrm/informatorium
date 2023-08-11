---
tags:
 - contest
 - easy
 - bfs
---

Дано два бинарных дерева с корнями `p` и `q`, необходимо определить являются ли эти деревья идентичными.

Два бинарных дерева можно считать **идентичными** если:

- Оба дерева имеют идентичную структуру;
- Соответствующие узлы имеют одинаковые значения.
##  Решение

Для решения данной задачи на графах отлично подойдёт алгоритм обхода дерева в ширину.

На каждом уровне для обоих деревьев мы будем сравнивать все узлы начиная слева на право, если ТОЛЬКО один из узлов отсутствует или соответствующих узлов отличаются, то мы можем считать деревья **неидентичными** и на этом завершить обход вернув `False`.

В случае если оба узла отсутствуют  (равны `None`), мы можем перейти ко следующей итерации в очереди.

Если оба узла существуют и их значения равны друг другу, мы можем добавить их дочерние узлы слева и справа с одинаковом порядку — левые узлы, а затем правые узлы первого и второго дерева соответственно.

```Python
@dataclass
class TreeNode:
	val: int
	left: Optional[TreeNode]
	right: Optional[TreeNode]
```

```Python
def is_same_tree(p: Optional[TreeNode], q: Optional[TreeNode]) -> bool:
	queue = deque([(p, q)])
	while queue:
		left, right = queue.popleft()

		if left is None and left == right:
			continue
		elif (
			left is None and left != right or
			right is None and right != left or
			left.val != right.val
		):
			return False

		queue.append((left.left, right.left))
		queue.append((left.right, right.right))

	return True
```

## Список источников

- [100. Same Tree](https://leetcode.com/problems/same-tree/description/)