# pywat
Python wats

A "wat" is a counterintuitive edge case shown with a snippet of code. The wat has a surprising result, and if you're not familiar with the language, you might conclude that the language is poorly designed when you see it. Often, more context about the language design will make the wat seem reasonable, or at least justified. In any case, the edge cases you see in wats rarely cause actual bugs, simply because the edge cases that actually come up during development are well-known, and therefore not very surprising.

PHP and JavaScript get their fair share of wats, but 

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
    >>> False / True == False
    True
    >>> True << False == True
    True

### Argument expansion

    >>> x = 1,2,3
    >>> min(x) == min(*x)
    True
    >>> y = (0,),
    >>> min(y) == min(*y)
    False

### Iterable types in comparisons

    >>> [1,2,3] == sorted([1,2,3])
    True
    >>> (1,2,3) == sorted((1,2,3))
    False

### Associative multiplication

    >>> x, y, z = "a", -1, -1
    >>> x * (y * z) == (x * y) * z
    False

### extend vs +=

    >>> a = ([],)
    >>> a[0].extend([1])
    >>> a
    ([1],)
    >>> a[0] += [2]
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
    TypeError: 'tuple' object does not support item assignment
    >>> a
    ([1, 2],)

### Comparing sets

    >>> x, y = {0}, {1}
    >>> sorted((x, y)) == sorted((y, x))
    False

### Indexing with floats

    >>> {0:4, 1:5, 2:6}[1]
    5
    >>> {0:4, 1:5, 2:6}[1.0]
    5
    >>> [4,5,6][1]
    5
    >>> [4,5,6][1.0]
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
    TypeError: list indices must be integers, not float
