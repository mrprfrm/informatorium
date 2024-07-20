---
authors:
  - Anton Petrov
status: Draft
tags:
  - context
  - medium
  - math
---
## Пример решения

```python
def min_flips(a, b, c):
	count = 0
    while a or b or c:
		a, ar = divmod(a, 2) if a else (0, 0)
		b, br = divmod(b, 2) if b else (0, 0)
		c, cr = divmod(c, 2) if c else (0, 0)

		if ar != br and cr == 0:
			count += 1
		elif ar == br and ar != cr:
			count += 2 if ar == 1 else 1

	return count
```

## Оптимизация

![[minimum_flips_to_make_a_or_b_equal_c.png]]

Также мы можем вычислить количество бит что нам необходимо поменять используя побитовые операции.

Сперва определим количество единиц для условия:

$$
a_{i} = 1 \lor b_{i} = 1 \land c_{i} = 0
$$

Для этого возьмём дизъюнкцию от `a` и `b` и `XOR` от `c`:

$$
a \lor b \oplus c
$$

Таким образом, на месте где `a == 1 or b == 1 and c == 0` в результате будут фигурировать единицы.

Далее определим количество единиц для условия:

$$
a_{i} = 1 \land b_{i} = 1 \land c = 0
$$

Для этого возьмём конъюнкцию от `a`, `b` и отрицания от `c`:

$$
a \land b \land \overline{c}
$$

Здесь, на месте где `a == 1 and b == 1 and c == 0` будут фигурировать единицы.

В итоге общее количество изменений что нам нужно произвести будет равно сумме единиц для `a == 1 or b == 1 and c == 0` и `a == 1 and b == 1 and c == 0`

```Python
def min_flips(a, b, c):
	return (bin((a | b) ^ c) + bin(a & b & ~c)).count('1')
```

## Список источников

- [1318. Minimum Flips to Make a OR b Equal to c](https://leetcode.com/problems/minimum-flips-to-make-a-or-b-equal-to-c/description/)
- [# Bitwise Operators in Python](https://realpython.com/python-bitwise-operators/)