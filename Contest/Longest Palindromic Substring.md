---
tags:
 - contest
 - medium
 - dp
 - palindrome
---

![[longest_palindromic_substring_diagonal.png]]

Дана строка `s`, необходимо определить длину самой продолжительной подстроки-палиндрома.

> Палиндром — это строка, где прядок букв справа на лево и с лева на право одинаковый.

```Python
"nnann" # Самый длинный палиндром nnann длинной 5 букв
"pkaak" # Самый длинный палиндром kaak длинной 4 буквы
"kadad" # Самый длинный палиндром ada длинной 3 буквы
"acaba" # Самый длинный палиндром aca длинной 3 буквы
```

## Решение

Исходя из определения палиндрома, решение можно свести к поиску самой продолжительной общей подстроки между `s` в прямом и `s` в обратном порядке, которая является палиндромом.

> Дело в том, что начало и конец исходной строки могут совпадать и являться частью более крупного палиндрома или же просто удовлетворять условию: 
>
> $x_{1}$ = $y_{1}$, ... , $x_{n-1}$ = $y_{n-1}$, $x_{n}$ = $y_{n}$ 
>
> где `x` — последовательность букв строки `s` в прямом порядке, `y` — последовательность букв строки `s` в обратном порядке.
>
> При этом сами обнаруженные подстроки палиндромом являться не будут.
>
> Поэтому, каждый раз находя более продолжительную общую подстроку, нам необходимо проверять её на палиндром.

![[longest_palindromic_substring_exception.png]]

## Пример решения

```Python
import math

def longest_palindrome(s):
	n = len(s)
	prev = [0] * n
	max_len, result = 1, ""
	
	for i in range(n):
		current = [0] * n
		for j in range(n):
			if s[j] == s[n - i - 1]:
				d = 1 if i < 1 or j < 1 else prev[j - 1] + 1
				current[j] = d
				
				if d > max_len:
					substring = s[j - d + 1:j + 1]
					if substring == substring[::-1]:
						result, max_len = substring, d
		prev = current
return result or s[0]
```

## Оптимизация

Следует заметить, что нам совершенно не обязательно находить все общие подстроки для `x` и `y`, нам достаточно найти только те что являются палиндромом.

Для это для каждого $s_{i}$ мы будем проверять соседние соседние элементы до тех пор пока:
$s_{i-j}$ = $s_{i+j}$

где `j` — это номер соседнего элемента от $s_{i}$.

![[longest_palindromic_substring_optimal.png]]

```Python
def get_substring(left, right, s, n):
	result = ""
	while left >= 0 and right < n and s[left] == s[right]:
		result = s[left:right + 1]
		left -= 1
		right += 1
	return result

def longest_palindrome(s):
	n = len(s)
	result = ""
	
	for i in range(0, n - 1):
		substring1 = get_substring(i, i, s, n)
		substring2 = get_substring(i, i + 1, s, n)
		result = substring1 if len(substring1) > len(result) else result
		result = substring2 if len(substring2) > len(result) else result
	
	return result or s[0]
```

## Список источников

- [5. Longest Palindromic Substring](https://leetcode.com/problems/longest-palindromic-substring/)