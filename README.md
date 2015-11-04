# Python wats

A "wat" is what I call a snippet of code that demonstrates a counterintuitive edge case of a programming language. The name comes from [this excellent talk by Gary Bernhardt](https://www.destroyallsoftware.com/talks/wat). If you're not familiar with the language, you might conclude that it's poorly designed when you see a wat. Often, more context about the language design will make the wat seem reasonable, or at least justified.

Wats are funny, and learning about a language's edge cases probably helps you with that language, but I don't think you should judge a language based on its wats. (It's perfectly fine to judge a language, of course, as long as it's an *informed* judgment, based on whether the language makes it difficult for its developers to write error-free code, not based on artificial, funny one-liners.) This is the point I want to prove to Python developers.

If you're a Python developer reading these, you're likely to feel a desire to put them into context with more information about how the language works. You're likely to say "Actually that makes perfect sense if you consider how Python handles *X*", or "Yes that's a little weird but in practice it's not an issue." And I completely agree. Python is a well designed language, and these wats do not reflect badly on it. You shouldn't judge a language by its wats.

## The Python wat quiz

Are you unimpressed by these wats? Do you think these edge cases are actually completely intuitive? Try your hand at [the Python wat quiz](https://github.com/cosmologicon/pywat/blob/master/quiz.md)!

## The wats

### Converting to a string and back

```python
>>> bool(str(False))
True
```

### Mixing integers with strings

```python
>>> int(2 * 3)
6
>>> int(2 * '3')
33
>>> int('2' * 3)
222
```

### The undocumented [converse implication](https://en.wikipedia.org/wiki/Converse_implication) operator

```python
>>> False ** False == True
True
>>> False ** True == False
True
>>> True ** False == True
True
>>> True ** True == True
True
```

### Mixing numerical types

```python
>>> x = (1 << 53) + 1
>>> x + 1.0 < x
True
```

Note: this is not simply due to floating-point imprecision.

```python
>>> x = float((1 << 53) + 1)
>>> x + 1.0 < x
False
```

Other languages that have floats and ints do not have this behavior. It is specific to Python.

Also, your systems may give different results, but you can always find at least one wat along these lines.

### Operator precedence?

```python
>>> False == False in [False]
True
```

[Source](https://www.reddit.com/r/programming/comments/3cjjgp/why_does_return_the_string_10/csxak65).

### Iterable types in comparisons

```python
>>> a = [0, 0]
>>> (x, y) = a
>>> (x, y) == a
False
```

```python
>>> [1,2,3] == sorted([1,2,3])
True
>>> (1,2,3) == sorted((1,2,3))
False
```

### Types of arithmetic operations

The type of an arithmetic operation cannot be predicted from the type of the operands alone. You also need to know their value.

```python
>>> type(1) == type(-1)
True
>>> 1 ** 1 == 1 ** -1
True
>>> type(1 ** 1) == type(1 ** -1)
False
```

### Fun with iterators

```python
>>> a = 2, 1, 3
>>> sorted(a) == sorted(a)
True
>>> reversed(a) == reversed(a)
False
```

```python
>>> b = reversed(a)
>>> sorted(b) == sorted(b)
False
```

### Circular types

```python
>>> isinstance(object, type)
True
>>> isinstance(type, object)
True
```

[Source](https://www.reddit.com/r/Python/comments/3c344g/so_apparently_type_is_of_type_type/csrwwyv).

### `extend` vs `+=`

```python
>>> a = ([],)
>>> a[0].extend([1])
>>> a[0]
[1]
>>> a[0] += [2]
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: 'tuple' object does not support item assignment
>>> a[0]
[1, 2]
```

### Indexing with floats

```python
>>> [4][0]
4
>>> [4][0.0]
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: list indices must be integers, not float
>>> {0:4}[0]
4
>>> {0:4}[0.0]
4
```

### `all` and emptiness

```python
>>> all([])
True
>>> all([[]])
False
>>> all([[[]]])
True
```

### `sum` and strings

```python
>>> sum("")
0
>>> sum("", ())
()
>>> sum("", [])
[]
>>> sum("", {})
{}
>>> sum("", "")
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: sum() can't sum strings [use ''.join(seq) instead]
```

### Comparing `NaN`s

```python
>>> x = 0*1e400  # nan
>>> len({x, x, float(x), float(x), 0*1e400, 0*1e400})
3
>>> len({x, float(x), 0*1e400})
2
```

### Variable shadowing before assignment

    >>> def f(n):
    ...   return n
    ... 
    >>> def main():
    ...   print f(4)
    ...   f = lambda x: x
    ... 
    >>> main()
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
      File "<stdin>", line 2, in main
    UnboundLocalError: local variable 'f' referenced before assignment
