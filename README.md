# Python wats

A "wat" is what I call a snippet of code that demonstrates a counterintuitive edge case of a programming language. The name comes from [this excellent talk by Gary Bernhardt](https://www.destroyallsoftware.com/talks/wat). If you're not familiar with the language, you might conclude that it's poorly designed when you see a wat. Often, more context about the language design will make the wat seem reasonable, or at least justified.

Wats are funny, and learning about a language's edge cases probably helps you with that language, but I don't think you should judge a language based on its wats. (It's perfectly fine to judge a language, of course, as long as it's an *informed* judgment, based on whether the language makes it difficult for its developers to write error-free code, not based on artificial, funny one-liners.) This is the point I want to prove to Python developers.

If you're a Python developer reading these, you're likely to feel a desire to put them into context with more information about how the language works. You're likely to say "Actually that makes perfect sense if you consider how Python handles *X*", or "Yes that's a little weird but in practice it's not an issue." And I completely agree. Python is a well designed language, and these wats do not reflect badly on it. You shouldn't judge a language by its wats.

### Converting to a string and back

    >>> bool(str(False))
    True

### Mixing integers with strings

    >>> int(2 * 3)
    6
    >>> int(2 * '3')
    33
    >>> int('2' * 3)
    222

### Arithmetic with booleans

    >>> False ** False == True
    True
    >>> True << False == True
    True

### Mixing numerical types

    >>> x = (1 << 53) + 1
    >>> x + 1.0 < x
    True

Note: this is not simply due to floating-point imprecision.

    >>> x = float((1 << 53) + 1)
    >>> x + 1.0 < x
    False

Also, different systems may have different results, but you can always find at least one wat along these lines.

### Argument expansion

    >>> x = 1,2,3
    >>> min(x) == min(*x)
    True
    >>> y = (0,),
    >>> min(y) == min(*y)
    False

### Iterable types in comparisons

    >>> a = [0, 0]
    >>> (x, y) = a
    >>> (x, y) == a
    False

    >>> [1,2,3] == sorted([1,2,3])
    True
    >>> (1,2,3) == sorted((1,2,3))
    False

### Types of arithmetic operations

The type of an arithmetic operation cannot be predicted from the type of the operands alone. You also need to know their value.

    >>> type(1) == type(-1)
    True
    >>> 1 ** 1 == 1 ** -1
    True
    >>> type(1 ** 1) == type(1 ** -1)
    False

### Substrings and containing

    >>> x, y = "acb", "cb"
    >>> y > max(x) and y in x
    True

### Associative multiplication

    >>> x, y, z = "a", -1, -1
    >>> x * (y * z) == (x * y) * z
    False

### `extend` vs `+=`

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

### Comparing sets

    >>> x, y = {0}, {1}
    >>> min((x, y)) == min((y, x))
    False

### Indexing with floats

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

### `all` and emptiness

    >>> all([])
    True
    >>> all([[]])
    False
    >>> all([[[]]])
    True

### `sum` and strings

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

### Comparing `NaN`s

    >>> x = 0*1e400  # nan
    >>> len({x, x, float(x), float(x), 0*1e400, 0*1e400})
    3
    >>> len({x, float(x), 0*1e400})
    2

