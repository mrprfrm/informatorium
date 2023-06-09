---
tags:
 - contest
 - medium
 - stack
---

![[vowels_counter.png]]

Дана строка `s` и целое число `k`, необходимо вернуть максимальное количество гласных в любой подстроке `s` длинной `k`.

```Python
"abciiidef", 3 # Подстрока "iii" содержит 3 гласные буквы
"aeiou", 2     # Подстрока "ae" содержит 2 гласные буквы
"queens", 3.   # Подстрока "uee" содержит 3 гласные буквы
"anno", 4.     # Подстрока "anno" содержит 2 гласные буквы
```

## Решение

Для подсчёта всех гласных внутри `s` мы преобразуем гласные и согласные в единицы и нули соответственно. Таким образом количество гласных в подстроке будет равно сумме всех элементов внутри подстроки, фактически решение сводиться к нахождению суммы всех нулей и единиц в заданном диапазоне.

Данный расчёт мы будем вести до тех пор пока не обойдём все возможные подстроки в `s`. Каждая последующая подстрока будет представлять из себя срез от начала `s` до `k`. 

На каждой итерации `s` будет уменьшаться на один элемент слева, таким образом на последней итерации `s` будет иметь длинну равную `k`, из чего можно сделать вывод, что все предыдущие подстроки были обработаны и цикл может быть завершён.

Также в случае, если количество гласных в подстроке будет равно `k` мы можем считать, что нашли максимально возможное количество гласных в подстроке и завершить поиск, вернув найденое значение в качестве ответа. 

## Примет решения

```Python
def max_vowels(self, s, k):
	max_count = 0
	vowels = {"a", "e", "i", "o", "u"}
	letters = [int(i in vowels) for i in s]
	
	while len(letters) >= k:
		count = sum(letters[:k])
		
		if count == k:
			return count
		
		max_count = max(max_count, count)
		letters = letters[1:]

return max_count
```

К сожалению, данное решение не является достаточно оптимальным, поскольку на каждой итерации список `letters` переопределяется через срез. Данная операция явно зависит от количества элементов в массиве и на больших объёмах данных такое решение просто не пройдёт проверку по времени.

## Оптимизация

Исходя из неэффективности использования срезов, заменим их на перерасчёт максимального количества элементов по их индексам.

Мы двигаемся по массиву последовательно, элемент за элементом, исходя из чего разница между предыдущим и последующим количеством гласных в подстроках будет отличаться на разность первого и последнего элемента.

Мы можем определить сумму гласных ещё до того как попадём в цикл: 

$$
sum_{0} = s_{0} + s_{1} ... s_{k-1} + s_{k}
$$

И уже затем расчитывать сумму гласных каждой последующей итерации на основе предыдущей:

$$
sum_{i} = sum_{i-1} - s_{i} + s_{k + i}
$$

```Python
def max_vowels(self, s, k):
	vowels = {"a", "e", "i", "o", "u"}
	letters = [bool(x in vowels) for x in s]
	
	n, start, end = len(s), 0, k
	count = sum(letters[start:end])
	max_count = count
	
	while end < n:
		count = count - letters[start] + letters[end]
		if count == k:
			return k
		
		max_count = max(count, max_count)
		
		start += 1
		end += 1
		
	return max_count
```

Также заметим, что вызов функции `max` более затратная операция чем банальное сравнение максимумов, заменим её вызов условием.

```Python
def max_vowels(self, s, k):
	vowels = {"a", "e", "i", "o", "u"}
	letters = [bool(x in vowels) for x in s]
	
	n, start, end = len(s), 0, k
	count = sum(letters[start:end])
	max_count = count
	
	while end < n:
		count = count - letters[start] + letters[end]
		if count == k:
			return k

		if count > max_count:
			max_count = count
		
		start += 1
		end += 1
		
	return max_count
```

Напоследок затронем приведение логического типа к целому числу. Данная операция сильно дороже чем использование тернарного оператора. Заменим вызов функции `int` тернарным оператором.

```Python
def max_vowels(self, s, k):
        vowels = {"a", "e", "i", "o", "u"}
        letters = [1 if x in vowels else 0 for x in s]
        
        n, start, end = len(s), 0, k
        count = sum(letters[start:end])
        max_count = count
        
        while end < n:
            count = count - letters[start] + letters[end]
            if count == k:
                return k
                
            if count > max_count:
                max_count = count
            
            start += 1
            end += 1
            
        return max_count
```

## Список источников

- [1456. Maximum Number of Vowels in a Substring of Given Length](https://leetcode.com/problems/maximum-number-of-vowels-in-a-substring-of-given-length/)