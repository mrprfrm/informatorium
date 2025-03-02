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

In Python, integers are represented by the `int` type. It supports all the basic arithmetic operations between numeric types but can also participate in operations with certain non-numeric types. Numbers of type `int` are represented by any integer literal[^1].

First, let's take a look at basic arithmetic operations with integers in Python.

```python
# Expression which contains basic arithmetic operations
2 + 2 * 6 / 3 - 4  # returns 2.0
```

> It's important to note that the division operation (`/`) between two integers returns a floating-point number. If we need an integer result, we can use the floor division operator (`//`).

The order of arithmetic operations in Python follows standard mathematical rules. If the default precedence is not suitable, parentheses can be used to alter the order of evaluation.

```python
# Expression with a specific order of operations
(2 + 2) * 6 / (3 - 4)  # returns -24.0
```

Since everything in Python is an object, each object that supports arithmetic operators must implement the appropriate methods. `int` is no exception. Below is an example of these methods in a custom class.

```python
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

Next, let's explore the remaining operations supported by the `int` type: floor division (`//`), modulo operation (`%`), and exponentiation (`**`).

```python
# Floor division returns the integer part of the division
17 // 3  # returns 5

# Modulo operation returns the remainder of the division
17 % 3  # returns 2

# Square of 5 expression
5 ** 2  # returns 25

# 2 raised to the power of 7
2 ** 7  # returns 128

# Square root of 16
16 ** 0.5  # returns 4.0
```

Just like basic arithmetic operations, the above operations are implemented through specific methods in the int class. Here’s how they can be defined in a custom class.

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

Python also supports operations between integers and some non-integer types.

```python
"repeatme" * 5
# returns 'repeatmerepeatmerepeatmerepeatmerepeatme'

(1, 2, 3, 4, 5) * 3
# returns (1, 2, 3, 4, 5, 1, 2, 3, 4, 5, 1, 2, 3, 4, 5)
```

For these cases, Python provides specific methods, which can also be implemented in a custom class.

```python
class CustomInteger:
    def __rmul__(self, other):
        # Implement the multiplication operation with the right operand
        return other * self._value
```

> These methods handle reflected or swapped operands only when the left operand does not support the operation.

## Number types casting

In Python, when arithmetic operations involve different numeric types, type conversion occurs according to the following rules:

- If one operand is complex, the result is complex.
- If one operand is floating-point, the result is floating-point.
- Otherwise, both operands remain integers.

> These rules apply only to operations whose documentation explicitly states that "the numeric arguments are converted to a common type."

**Implicit type casting** happens automatically, while **explicit type casting** requires calling methods or functions. The `int()` function can be used to convert appropriate types to integers. A custom type must implement the `__int__()` method.

```python
class CustomInteger:
    def __int__(self):
        # Cast the custom type to an integer, must return an integer value
```

> The `__int__` method is required for casting custom types to `int`. If it does not return an integer, a `TypeError` is raised.

Python provides built-in types that can be converted to `int`:

```python
# The proper integer literal
int(314e-2)  # returns 3

# The string containing a proper integer literal
int("4815")  # returns 4815

# The floating-point number
int(12.7)  # returns 12

# The boolean value
int(True)  # returns 1
```

> During conversion to an integer, floating-point numbers are truncated, not rounded.

The `int()` function also supports an optional `base` argument.

```python
int("A5", base=16)  # returns 165
int("0xa", base=0)  # returns 10
```

> Setting `base=0` enables automatic detection of the numeric base using common prefixes: `0b` for binary, `0o` for octal, and `0x` for hexadecimal. The default base is 10.

Python also supports integer conversion from `bytes` and `bytearray` using `from_bytes`.

```python
int.from_bytes(b'\x00\x10', byteorder='big')  # returns 16
int.from_bytes(b'\x00\x10', byteorder='little')  # returns 4096
```

## Logical operations with integers

Python supports logical operations on integers. These can be combined into logical expressions using and, or, and not.

```python
24 > 11.5 >= 11.5 or 3.14 == 3 != 4.0 and 12 < 24.75 <= 31.4
# returns True
```

> Complex numbers cannot be directly compared due to their mathematical nature.

Python provides special methods for logical comparisons. Any type implementing these methods can be used in logical expressions.

```python
class CustomInteger:
    def __lt__(self, other):
        # Implement less than

    def __le__(self, other):
        # Implement less than or equal

    def __eq__(self, other):
        # Implement equal

    def __ne__(self, other):
        # Implement not equal

    def __gt__(self, other):
        # Implement greater than

    def __ge__(self, other):
        # Implement greater than or equal
```

Python also supports instance comparison using the `is` and `is not` operators. These check whether two references point to the same object in memory.

```python
8 is 8  # returns True
8 is not 8.0  # returns True
```

> This type of comparison is only valid for objects of the same type and is generally more efficient than value-based comparisons.

Additionally, Python allows integers to be used directly in logical operations by implicitly casting them to the `bool` type.

```python
4 and True  # returns True
0 or False  # returns False
```

> Note that there is no `__not__()` method for object instances; only the interpreter core defines this operation. The result is affected by the `__bool__()` and `__len__()` methods.

## Functions for working with integers

Python provides built-in functions to work with integers. Some serve as optimized operator alternatives, while others provide unique capabilities.

### Absolute value

The `abs()` function returns the absolute value of a number.

```python
abs(-481)  # returns 481
```

It relies on the `__abs__()` method, which can be implemented in custom types.

```python
class CustomInteger::
    def __abs__(self):
        # Implement the absolute value operation
```

### Power

The `pow()` function returns the value of the first argument raised to the power of the second argument. It also accepts an optional third argument, `mod`, which applies a modulo operation to the result.

```python
pow(10, 2)  # returns 100
pow(10, -2)  # returns 0.01
pow(10, 2, 3)  # returns 1
```

> Note that `pow(10, 2)` is equivalent to `10 ** 2`, but `pow(10, 2, 3)` is more efficient than `10 ** 2 % 3`.

The function also relies on the appropriate `__pow__()` method. The method should be defined to accept an optional third argument if the ternary version of the `pow()` function is to be supported.

```python
class CustomInteger:
    def __pow__(self, other, mod=None):
        # Implement the power operation
```

There are also cases that cannot be handled by the `**` operator alone. One such case is modular inversion[^2].

```python
# Finding the modular inverse of 38 by modulo 97
pow(38, -1, mod=97)  # returns 23

# Verifying that 23 is the correct modular inverse
23 * 38 % 97 == 1  # returns True
```

Now, let's attempt the same operation using the `**` operator instead of `pow()`.

```python
38 ** -1 % 97  # returns 0.02631578947368421
```

> The result is quite unexpected. It happens because `38 ** -1` performs reciprocal division before the modular inversion.

### Modulo division

The `divmod()` function returns a tuple containing both the quotient and the remainder of a division. It is particularly useful for algorithms involving the greatest common divisor (GCD)[^3] or positional number systems.

```python
divmod(123, 60)  # returns (2, 3)
```

> The same result can be obtained using `quotient, remainder = a // b, a % b`, but `divmod()` is both more readable and more performant.

The `__divmod__()` method can be implemented in custom types to support the `divmod()` function.

```python
class CustomInteger:
    def __divmod__(self, other):
        # Implement the floor division and modulo operations simultaneously
```

## References

- [An Informal Introduction to Python (python.org)](https://docs.python.org/3/tutorial/introduction.html)
- [Built-in Types (python.org)](https://docs.python.org/3/library/stdtypes.html)
- [Built-in Functions (python.org)](https://docs.python.org/3/library/functions.html)
- [Lexical Analysis (python.org)](https://docs.python.org/3/reference/lexical_analysis.html)
- [Expressions (python.org)](https://docs.python.org/3/reference/expressions.html)
- [Numbers in Python (realpython.com)](https://realpython.com/python-numbers)

[^1]: An integer literal is a number, a letter, or a combination of letters and numbers that correspond to a specific numeral system, as well as the \_ symbol in number notation. For example: 1, 24, 0b01101, 0xC5, 10_000_000, and so on.

[^2]: A modular inverse of a number `a` modulo `m` is a number `x` such that `(x * a) % m == 1`, where `m` is the modulus. The Extended Euclidean Algorithm describes how to compute the modular inverse. Numbers with this property are essential in cryptography, particularly in algorithms like the Diffie-Hellman key exchange.

[^3]: The greatest common divisor (GCD) is the largest number that evenly divides two or more numbers without leaving a remainder. The Euclidean Algorithm is a widely used method for computing the GCD efficiently. This algorithm plays a crucial role in cryptography, including in the RSA encryption algorithm.
