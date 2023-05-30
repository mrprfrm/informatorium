---
tags:
 - contest
 - medium
 - recursion
---

Даны три массива `equations`, `values` и `queries`, где:

- `equations` — хранит набор пар вида `(a, b)`, где `a` и `b` — делимое и делитель соответственно;
- `values` — хранит частное `a` и `b`;
- `queries` — хранит набор выражений, что необходимо найти на основе известных выражений.

$$
values_{i} = equations_{i}[0] / equations_{i}[1]
$$

Необходимо найти значение для каждого выражения из `queries`, в случае если найти значение выражения невозможно следует вернуть `-1`.

```Python
equations = [["a","b"],["b","c"]]
values = [2.0,3.0]
queries = [["a","c"],["b","a"],["a","e"],["a","a"],["x","x"]]

# Значения выражений queries:
# a / c = 6
# b / a = 0.5
# a / e = -1
# a / a = 1
# x / x = -1
# Ответ [6, 0.5, -1, 1, -1]
```

## Решение

Рассмотрим пример выше.

Исходя из выражения `a / b = 2`, определим систему `a` и `b`:

$$
\left\{ 
\begin{array}{l}
a = 2 * b \\
b = a / 2 
\end{array}
\right.
$$

Аналогичным образом, исходя из выражения `b / c = 3`, определим систему для `b` и `c`:

$$
\left\{ 
\begin{array}{l}
b = 3 * c \\
c = b / 3
\end{array}
\right.
$$

Найдём значение выражения `a / c`, подставив значение согласно системам:

$$
a / c = 2b / \frac{b}{3} = 2b * \frac{3}{b} = \frac{6b}{b} = 6
$$

Найдём значение выражения `b / a`, аналогичным образом:

$$
b / a = \frac{b}{2b} = \frac{1}{2} = 0.5
$$

Выражение `a / e`, согласно условию будет равно `-1`, поскольку зависимость `e` от известных значений не определена.

Выражение `a / a` будет равно `1`, поскольку в данном случае мы делим выражение число само на себя.

Выражение `x / x`, так же будет равно `-1`, поскольку зависимость `x` от известных значений не определена.

> Следует заметить, что выражение `x / x` будет равно `-1` только исходя из условия задачи, на практике можно считать `x / x` равным `1` посокльку фактически мы делим `x` само на на себя.

## Пример решения

Сперва создадим список всех возможных выражений для каждого делимого и делителя на основе  `equations` и `values`

```Python
n = len(equations)
expressions = defaultdict(list)

for i in range(n):
	a, b = equations[i]
	value = values[i]
	expressions[a].append((b, value))
	expressions[b].append((a, 1 / value))
```

Затем определим рекурсивную функцию для непосредственной подстановки выражений в соответсвии с делителем и делимым. Исходя из вышеописанного решения, будем заменять делимое на все возможные вариации до тех пор пока в выражении не останется только одна неизвестная. 

Например, вычисляя значение выражения `b / a` из примера выше, мы имеем на руках выражения эквивалентные `a`:

$$
a = 2 * b
$$

Заменив `a` выражением выше мы получим выражение в котором есть только одна неизвестная — `b`, то есть делитель `b` равен делимому `b`, и как следствие эти неизвестные могут быть упразднены или заменены на единицу, после чего нам останется вычислить значение выражения:

$$
b / a = 1b / 2b = 1/2 = 0.5
$$

Также заметим, что если эквивалентные выражения для делителя или делимого не были определены, то такие выражения всегда будут равны `-1`.

```Python
def calc_equation(equations, values, queries):
	expressions = defaultdict(list)

	def dfs(a, b, visited):
		if not (a in expressions and b in expressions):
			return -1
		
		if a == b:
			return 1

		visited.add(a)
		for char, value in expressions[a]:
			if char not in visited:
				answer = dfs(char, b, visited)
				if answer != -1:
					return answer * value
		return -1
	
	n = len(equations)
	for i in range(n):
		a, b = equations[i]
		value = values[i]
		expressions[a].append((b, value))
		expressions[b].append((a, 1 / value))

	for a, b in queries:
		yield dfs(a, b, set())
```

## Оптимизация

Также мы можем обойтись без дополнительного извлечения `a` и `b` в теле цикла, добавив их в его определении. Для этого скомбинируем значения из `equations` и `values` в последовательность пар с помощью функции `zip`.

```Python
def calc_equation(equations, values, queries):
	expressions = defaultdict(list)

	def dfs(a, b, visited):
		if not (a in expressions and b in expressions):
			return -1
		
		if a == b:
			return 1

		visited.add(a)
		for char, value in expressions[a]:
			if char not in visited:
				answer = dfs(char, b, visited)
				if answer != -1:
					return answer * value
		return -1
	
	for (a, b), value in zip(equations, values):
		expressions[a].append((b, value))
		expressions[b].append((a, 1 / value))

	for a, b in queries:
		yield dfs(a, b, set())
```

## Список источников

- [399. Evaluate Division](https://leetcode.com/problems/evaluate-division/description/)