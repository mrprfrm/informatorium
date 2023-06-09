---
tags:
 - contest
 - easy
 - dp
---

Дано число `n`, необходимо определить количество единиц в двоичном представлении каждого числа от `0` до `n` включительно.

```Python
2 # [0, 1, 10] содержат [0, 1, 1] единиц
5 # [0, 1, 10, 11, 100, 101] содержат [0, 1, 1, 2, 1, 2] единиц
0 # [0] содержит [0] единиц
7 # [0, 1, 10, 11, 100, 101, 110, 111] содержат [0, 1, 1, 2, 1, 2, 2, 3] единиц
```

## Решение

Для каждого числа от `0`, до `n` определим двоичное представление как список нулей и единиц, а сумма всех элементов такого массива будет равна соответствующему количеству единиц для каждого элемента.

Двоичное представление можно определить как совокупность всех остатков от деления числа `n` на `2`.

## Пример решения

```Python
def get_bin(n):
	while n:
		n, r = divmod(n, 2)
		yield(r)

def count_bits(self, n):
	dp = [0, 1]
	for i in range(2, n + 1):
		count = sum(get_bin(i))
		dp.append(count)
	return dp if n > 0 else [0]
```

## Оптимизация

Разберём пример решения задачи для числа `5`.

![[counting_bits.png]]

В данном случае, количество единиц для числа `0` и числа `1` мы будем считать за основу решения. Далее можно заметить зависимость:

Для `n == 1`:

$$
dp_{1} = 0, 1
$$

Для каждого чётного `n`:

$$
dp_{n} = dp_{2/2}, dp_{3/2} + 1, ..., dp_{n-1/2} + 1, dp_{n/2}
$$

Для каждого нечётного `n`:

$$
dp_{n} = dp_{2/2}, dp_{3/2} + 1, ..., dp_{n-1/2}, dp_{n/2} + 1
$$

```Python
def count_bits(self, n):
	dp = [0] * (n + 1)
	for i in range(1, n + 1):
		if i % 2 == 0:
			dp[i] = dp[i // 2]
		else:
			dp[i] = dp[i // 2] + 1
	return dp
```

## Список источников

- [338. Counting Bits](https://leetcode.com/problems/counting-bits/)