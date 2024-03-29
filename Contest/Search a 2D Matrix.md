---
tags:
 - contest
 - medium
 - matrix
 - binarysearch
---


Даны число `target` и матрица `matrix` размеренностью `m x n`, где:

- Каждая строка отсортирована в неубывающем порядке;
- Каждый первый элемент следующей строки больше последнего элемента предыдущей строки.

Необходимо вернуть булевое `trrue` или `false`, в зависимости от наличия числа `target` в матрице `matrix`.

## Решение

Исходя из упорядоченности матрицы для решения задачи мы можем воспользоваться частной реализацией [[Двоичный поиск|двоичного поиска]]. 

Сперва определим строку где будем искать `target`, для этого будем сравнивать первый и последний элемент текущей строки, саму строку будем выбирать согласно алгоритму двоичного поиска — начиная с середины матрицы, определим ту часть строк, что потенциально может содержать искомое число, после чего повторять процесс отсечения до тех пор пока не найдём искомую строку.

После определения искомой строки аналогичным образом, используя алгоритм двоичного поиска, попробуем определить искомое число в найденной строке — начиная с середины строки, определим ту её часть, что потенциально может содержать искомое число, процесс отсечения будем продолжать до тех пор пока не найдём искомое число в строке.

В случае если не удалось определить строку, потенциально содержащую искомое число или само число не было найдено в такой строке — возвращаем `false` в противном случае возвращаем `true` сразу после нахождения числа.

```Python
def search_matrix(matrix: List[List[int]], target: int) -> bool:
	row = []
	start, end = 0, len(matrix)
	while start < end:
		mid = min(end-1, end // 2 + start)
		if matrix[mid][0] > target:
			end = mid
		elif matrix[mid][-1] < target:
			start = mid + 1
		else:
			row = matrix[mid]
			break
	
	start, end = 0, len(row)
	while start < end:
		mid = min(end - 1, end // 2 + start)
		if row[mid] > target:
			end = mid
		elif row[mid] < target:
			start = mid + 1
		else:
			return True
		
	return False

```

## Оптимизация

В целом, после обнаружения искомой строки в матрице можно воспользоваться оператором `in` который может определить наличие элемента внутри коллекции, однако стоит помнить, что для списков (и вообще всех стеков) такой поиск будет происходить линейно и будет зависеть от позиции элемента и общего количества элементов в списка, однако сама операция является низкоуровневой, что может позволить нам сэкономить на затратах по памяти. 

```Python
def searchMatrix(self, matrix: List[List[int]], target: int) -> bool:
        start, end = 0, len(matrix)
        while start < end:
            mid = min(end-1, end // 2 + start)
            if matrix[mid][0] > target:
                end = mid
            elif matrix[mid][-1] < target:
                start = mid + 1
            else:
                return target in matrix[mid]
        
        return False
```
## Список источников

- [74. Search a 2D Matrix (leetcode.com)](https://leetcode.com/problems/search-a-2d-matrix/)