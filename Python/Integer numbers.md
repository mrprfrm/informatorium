---
authors:
  - Anton Petrov
status: draft
tags:
  - python
  - numbers
---
Numbers and operations with them are the basis of any programming language. Python is no exception. Python supports a wide range of numeric types, including integers, floating-point numbers, and complex numbers. In this note, we will consider the basic operations with integer numbers, as well as some of the features of working with them. 

## Integer numbers basics

In python integers are represented by the `int` type. It supports all the basic arithmetic operations between basic numeric types, but also can participate in operations between some other non-numeric types in Python. Numbers of type `int` are represented by any integer literal.

First, let's take a look to a basic arithmetic operations with integers in Python. 

```Python
# Expression which contains basic arithmetic operations
2 + 2 * 6 / 3 - 4 # returns 2.0
```

> It's important to note that the division operation (`/`) between two integers returns a floating-point number. If we need to get an integer result, we can use the floor devision operator (`//`). 

Arithmetic operations order in python is following the standard mathematical rules. And for those cases when the standard order is not suitable for particular needs we can change it by using parentheses. 

```Python
# Expression with a specific order of operations
(2 + 2) * 6 / (3 - 4) # returns -24.0 
```

As everithing in Python is an object, each object, which supporting basic arithmetic operators, should implement the appropriate methods. `int` is not an exception. Now let's take a closer look to these methods in a custom class implementation. 

```Python 
class CustomInteger:
    def __add__(self, other):
        # Implement the addition operation

    def __sub__(self, other):
        # Implement the subtraction operation

    def __mul__(self, other):
        # Implement the multiplication operation

    def __truediv__(self, other):
        # Implement the regular division operation
```

Next, let's dive into the remaining operations supported by `int` type: floor division (`//`), modulo operation (`%`) and power operation (`**`).

```python
# Floor division returns the integer part of the division
17 // 3  # returns 5 

# Modulo operation returns the remainder of the division
17 % 3 # returns 2 

# Sqaure of 5 expression
5 ** 2 # returns 25 

# 2 in power of 7 expression
2 ** 7 # returns 128 

# Square root of 16
16 ** 0.5 # returns 4.0  
```

Just as basic arithmetic operations, the operations above are implemented by the appropriate methods in the `int` class. Now, let's pay attention to these methods in a custom class implementation as well. 

```Python 
class CustomInteger:
    def __floordiv__(self, other):
        # Implement the floor division operation 
        return self._value // other

    def __mod__(self, other):
        # Implement the modulo operation 
        return self._value % other

    def __pow__(self, other):
        # Implement the power operation and pow function
        return self._value ** other
```

Also `int` in Python supports an operations between some non-integer types. 

```Python
"repeadme" * 5 
# returns repeadmerepeadmerepeadmerepeadmerepeadme
(1, 2, 3, 4, 5) * 3
# returns (1, 2, 3, 4, 5, 1, 2, 3, 4, 5, 1, 2, 3, 4, 5)
```

For these particular examples Python also provides the appropriate methods. They are different from the methods for basic arithmetic operations, but still can be implemented in a custom class. 

```Python
class CustomInteger:
    def __rmul__(self, other):
        # Implement the multiplication operation with the right operand 
        return other * self._value
```

> The methods like this are implemented for **reflected** or **swapped** operands. They are used when the left operand does not support the corresponding operation. 

## Numbers types casting

In Python, when arithmetic operations are performed between numbers with different numeric types, these numbers are casting to the common type following the rules:

- If one of the operands is a complex number, the second will be cast to a complex number; 
- If one of the operands is a floating-point number, the second will be cast to a floating-point number; 
- If any other cases both operands are integers and no casting required.

> It's important to note that the rules above works only for those operators which description uses the phrase "the numeric arguments are converted to a common type".

The types casting during arithmetic operations can be considered as **implicit types casting**, which means that the casting happens automatically without any additional operations in code. **Explicit types casting** requires the explicit calls of appropriate methods or functions to convert one type to another. 

The explicit casting to integer in Python can be done using the `int()` function. This function can convert any appropriate type to an integer, the following type should implement the `__int__` method.

```Python
class CustomInteger:
    def __int__(self):
        # Cast the custom type to integer, should return integer value
```

> `__int__` method is required for custom type custing to `int`. It can contain any logic, but always should return integer value, otherwise the `TypeError` will be raised. 

Python has couple built-in types that can be casted to integer. For instance

```Python
# The proper integer literal
int(314e-2) # returns 3

# The string contains proper integer literal
int("4815") # returns 123

# The floating-point number
int(12.7) # returns 12

# The boolean value
int(True) # returns 1
```

> During the casting to integer, the floating-point numbers are truncated to the integer part.

Also `int` function can take one optional arguments: `base`, which allows to set the base of the number during the casting. 

```python
int("A5", base=16) # returns 165
int("0xa", base=0) # returns  10
```

> `base` equal to `0` means that the base will be determined by the prefix of the string: `0b` for binary, `0o` for octal, `0x` for hexadecimal, and the default base is `10`.

And of cource, Python supports casting to integer for `bytes` and `bytearray` types, using method `from_bytes`. 

```Python
int.from_bytes(b'\x00\x10', byteorder='big') # returns 16
int.from_bytes(b'\x00\x10', byteorder='little') # returns 4096
```

`byteorder` argument is responsible for the byte order of the number: `big` — for most significant byte first, `little` — for least significant byte first. 

## Logical operations with integers

Python supports logical operations with integer numbers. The numbers actually may have different types and result of these operations might be combined into a logical expressions using logical operators: `and`, `or`, `not`.

```Python
24 > 11.5 >= 11.5 or 3.14 == 3 != 4.0 and 12 < 24.75 <= 31.4
# returns True
```

> Let's note that not all numbers types support logical operations. For instance, complex numbers might be compared only by module, which is quite logical, considering the nature of imaginary unit.

For all these operations Python provides the appropriate methods. Any type implementing these methods can be used in logical expressions. 

```Python
class CustomInteger:
    def __lt__(self, other):
        # Implement the less than operation

    def __le__(self, other):
        # Implement the less than or equal operation

    def __eq__(self, other):
        # Implement the equal operation

    def __ne__(self, other):
        # Implement the not equal operation

    def __gt__(self, other):
        # Implement the greater than operation

    def __ge__(self, other):
        # Implement the greater than or equal operation
```

Also, Python supports comparison between instances by their references using operators `is` and `is not`. 

```Python
8 is 8 # returns True
8 is not 8.0 # returns True
```

> In this case the comparison is done by the references of the objects. The comparison like this is fair only for numbers of the same type and usually is more performance efficient.

Python also allows `int` type to participate in logical operations directly. In this case, the integer is casted to the `bool` type. 

```Python
4 and True # returns True
0 or False # returns False
```

> Note that there is no `__not__()` method for object instances; only the interpreter core defines this operation. The result is affected by the `__bool__()` and `__len__()` methods.

## Functions for working with integers

Python provides a set of built-in functions for working with numbers types. Some of them are just a more performant alternative to operators, others provide d unique functionality unavailable with operators.

### Absolute value

The `abs()` function returns the absolute value of a number. Let's have an example. 

```Python
abs(-481) # returns 481
```

The `abs` function can be used with any type, which implements the `__abs__` method. 

```python
class CustomComplex:
	def __init__(self, rel, img):
		self._rel = rel
		self._img = img
	
	def __abs__(self):
		return (self._rel ** 2 + self._img ** 2) ** 0.5
```

### Power

The `pow()` function returns the value of the first argument raised to the power of the second argument. It also can take the third optional argument `mod`, which can be used to calculate the modulo of the result. 

```Python
pow(10, 2) # returns 100
pow(10, -2) # returns 0.01
pow(10, 2, 3) # returns 1
```

> Note that `pow(10, 2)` is equivalent to `10 ** 2` but `pow(10, 2, 3)` is more performant than `10 ** 2 % 3`. 

There is also a cases which simply can not be resolved with `**` operator. For instance let's take a closer look to modular inversion operation. 

```Python
# Let's find the inverse number of 38 by modulo 97
pow(38, -1, mod=97)  # returns 23

# Now, let's check if 23 actually is the inverse number
23 * 38 % 97 == 1 # returns True
```

Now, let's try the same with operators instead of function. 

```Python
38 ** -1 % 97 # returns 0.02631578947368421
```

The result is different and is not satisfying expectations. The reason is that `38 ** -1` performs the reciprocal devision before the modular inversion. 

### Modulo devision

The `divmod()` function returns the tuple of two values: the result of the floor division and the remainder of the division. It's perforct for greatest common divisor calculation. 

```Python
divmod(123, 60) # returns (2, 5)
```

> We can rich the same result using the appropriate operators: `quotient, remainder = a // b, a % b`, but `divmod` is not only more readable but also more performant.

## Sources

- [An Informal Introduction to Python (python.org)](https://docs.python.org/3/tutorial/introduction.html)
- [Built-in Types (python.org)](https://docs.python.org/3/library/stdtypes.html)
- [Built-in Functions (python.org)](https://docs.python.org/3/library/functions.html#built-in-functions)
- [Lexical analysis (python.org)](https://docs.python.org/3/reference/lexical_analysis.html)
- [Expressions (python.org)](https://docs.python.org/3/reference/expressions.html)
- [Numbers in Python (realpython.com)](https://realpython.com/python-numbers)
