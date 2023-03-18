---
tags:
 - python
 - syntax
---

## TODO

- [ ] Literal pattern check
- [ ] Capture pattern check
- [ ] Wildcard pattern check
- [ ] Constant pattern check
- [ ] Sequence pattern check
- [ ] Mapping pattern check

## Literal Patterns

Данная группа паттернов предполагает буквальное сравнение входного значения с конкретным числом, словом, булевым значением или значением `None`.

```Python
match value:
	case 15:
		print("The value is number 15.")
	case "Some string":
		print("The value is string 'Some stiring'")
```

> TODO: подумать на счёт менее абстрактных примеров для  `literal` паттернов.

## Capture Patterns

Группа шаблонов захвата позволяет использовать имена переменных в контексте кейсов. Данной переменной будет присвоено значение переданное в  `match`.

```Python
name = "Anton"

match value:
	case "some value":
		print("Some value sting was passed.")
	case name:
		print(f"{name} name was passed")
```

> TODO: Также как и пример выше, здесь мы нуждаемся в менее абстрактных примерах.

## Widlcard Pattern

В данная группа позволяет нам определить кейс по-умолчанию, то есть данный кейс отработает в случае, если все другие определённые выше кейсы не подошли.

```Python
match number:
	case 42:
		print("This is the number 42!")
	case _:
		print("This is default case")
```

> TODO: Примеры, примеры и ещё раз примеры

## Constant Value Patterns

Данная группа позволяет использовать константы при определении кейсов, однако здесь следует отдельное вниманию уделять именам констант — константы должны именоваться в верхнем регистре, согласно `PEP8`, в противном случае будет иметься в виду `Capture Pattern`.

```Python
OK_STATUS = 201
BAD_REQUEST_STATUS = 400

response = {status: 400, data: {"name": ["Field 'Name' is required"]}}

match response["status"], response["data"]:
	case OK_STATUS, msg:
		print("Item successfully created.")
	case BAD_REQUEST_STATUS, errors:
		print("Request contains invalid: f{errors}")
	case _:
		print("Invalid response signature")
```

## Sequence Pattern

Данная группа позволяет сопоставлять коллекции образованные от `collections.abc.Sequence`, всё за исключением строк (`str`), байтовых последовательностей (`bytes`) и байтовых массивов (`bytesarray`)

```Python
queue = [12]

match queue:
	case []:
		print("The queue is empty.")
	case [12]:
		ptint("This is exact the queue we expected.")
	case [x]:
		print("This is the queue with the same length we expected.")
	case [x, *]:
		print("This queue contains more elements than we expected.")
	case [12, *]:
		print("This queue contains 12 and other values.")
```

> Исключительно полезный паттерн, что позволяет сильно упростить процесс поиска элемента в массиве, проверка на длину массива происходит под капотом.

> TODO: возможно найдётся более удачный пример

## Mapping Patterns

Данная группа позволяет сопоставлять словари или любые иные объекты образованные от `collections.abc.Mapping`.

```Python
match value:
	case {"items": data}:
		print("This is the items list")
	case {"item": data}:
		print("This is the singe item")
	case {"error": msg}:
		print("This is the error message")
```

> Также следует заметить, что если в аргументе `value` встречается сразу несколько ключей подходящих нескольким кейсам, обработан будет первый попавшийся из них.

```Python
response = {"status": 200, "data": []}

match response:
	case {"status": status}:
		print(f"The status is {status}")
	case {"data": data}:
		print(f"The data is {data}")
```

> В данном случае обработан будет первый кейс, до второго интерпретатор доберётся только в случае если `status` в `response` отсутствует.

