# Python wat explanation

This page provides explanations for the details behind [the Python wats](https://github.com/cosmologicon/pywat/blob/master/explanation.md).


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

This is not actually an undocumented feature. `**` is simply the exponent operator, and `False` and `True` are simply the numbers `0` and `1`. In mathematics, 0^0 = 1, 0^1 = 0, etc. It just happens to work out that the truth table is the same as for the converse implication operator.

### Mixing numerical types

```python
>>> x = (1 << 53) + 1
>>> x + 1.0 < x
True
```

Let's call the real number N = 2^53 + 1 = 9007199254740993. Despite what you might have heard, floating-point representation does *exactly* represent a large range of integers, just like integer types do. (If that's surprising to you, see [this excellent paper by David Goldberg](https://ece.uwaterloo.ca/~dwharder/NumericalAnalysis/02Numerics/Double/paper.pdf).) N is significant in that it's the smallest whole number that is not represented exactly by a Python `float`, which has 52 bits of precision. Of course, N can be represented exactly by a Python `int`.

So let's look at the value that `x` holds. The right-hand side `(1 << 53) + 1` is an integer expression that's equal to N, so `x` gets the value N. If `x` is converted to a float with `float(x)`, then the result cannot be N exactly, since floats can't represent that value. According to rounding rules, the value gets converted to N - 1, which floats *can* represent.

So what is the value of `x + 1.0`? First, in order to perform the addition, `x` (which is an int containing the value N) is converted to a float. The result of this conversion is a float equal to N - 1. Next `1.0` is added, bringing the total value back to N. Finally, this value is coerced into a float again, bringing it back to N - 1. When `x + 1.0 < x` is evaluated, the left side is a float containing the value N - 1, and the right is an int containing the value N. The line is essentially this:

	9007199254740992.0 < 9007199254740993

There are three options that programming languages take when asked to perform this comparison. First, they may throw an error, as in Haskell, which refuses to compare floating-point types to integer types. Second, they may convert the right-hand side to float, in which case the right-hand side gets converted to N - 1, just like the left, and the comparison evaluates to false. This is the option taken by most popular languages, including C, C++, and Java.

Python and Ruby, however, take a third option, which involves more complicated logic, designed to catch exactly this case. In Python and Ruby, the result of a comparison will be the comparison of the actual numerical values, even if one side cannot be accurately represented with an integer, and the other side cannot be accurately represented with a float.

Again, this is not just floats being weird (and if you really think floats behave weirdly, I highly recommend that Golberg paper). This wat does not happen in most languages that use floats, and it doesn't happen when you stick with floats:

```python
>>> x = float((1 << 53) + 1)
>>> x + 1.0 < x
False
```

It only happens when you mix floats with ints, and only if you have Python or Ruby's complicated comparison logic.

### Operator precedence?

```python
>>> False == False in [False]
True
```

Neither the `==` nor the `in` happens first. They're both [comparison operators](https://docs.python.org/3.5/reference/expressions.html#comparisons), with the same precedence, so they're chained. The line is equivalent to `False == False and False in [False]`, which is `True`.

### Iterable types in comparisons

Lists and tuples do not compare equal to each other, even if they contain the same elements.

```python
>>> [] == ()
False
```

Some people have claimed to me that this is this is the way it should be, that different types are not supposed to compare equal to each other. While there may be good reasons for lists and tuples specifically to not compare equal, the general claim that different types should not compare equal is very un-pythonic, in my humble opinion. There are many instances of different types comparing equal in Python. If you really insist that this one's not a wat because list and tuple are different types, please accept these 13 wats instead:

```python
>>> True == 1
True
>>> True == 1.0
True
>>> True == 1j
True
>>> set([]) == frozenset([])
True
>>> u"" == ""  # Different types in Python 2 only.
True
>>> bytearray() == ""  # Only true in Python 2.
True
>>> collections.OrderedDict() == dict()
True
>>> collections.defaultdict() == dict()
True
>>> collections.Counter() == dict()
True
>>> collections.namedtuple("x", "a")(1) == (1,)
True
>>> collections.UserList() == []
True
>>> collections.UserString() == ""
True
>>> enum.IntEnum("x", "a").a == 1
True
```

### Fun with iterators

```python
>>> a = 2, 1, 3
>>> sorted(a) == sorted(a)
True
>>> reversed(a) == reversed(a)
False
```

Unlike `sorted` which returns a list, `reversed` returns an iterator. Iterators compare equal to themselves, but not to other iterators that contain the same values.

```python
>>> b = reversed(a)
>>> sorted(b) == sorted(b)
False
```

The iterator `b` is consumed by the first `sorted` call. Calling `sorted(b)` once `b` is consumed simply returns `[]`.

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

[Explanation from the Python FAQ.](https://docs.python.org/3/faq/programming.html#why-does-a-tuple-i-item-raise-an-exception-when-the-addition-works)

### `all` and emptiness

```python
>>> all([])
True
>>> all([[]])
False
>>> all([[[]]])
True
```

When converted too a bool, `[]` becomes `False` (since it's empty), and `[[]]` becomes `True` (since it's not empty). Therefore `all([[]])` is equivalent to `all([False])`, and `all([[[]]])` is equivalent to `all([True])`. The first case `all([])` is trivially `True`.

### NaN-related wats

```python
>>> x = float("nan")
>>> len({x, x, float(x), float(x), float("nan"), float("nan")})
3
>>> len({x, float(x), float("nan")})
2
```

For the purpose of set membership, two objects `x` and `y` are considered equivalent if `x is y or x == y`. For two NaNs, this will be true whenever they're the same object. So `{x, x}` will have length `1`. Every separately-defined NaN is a different object, so `x is 0*1e400` is `False`, and `{x, 0*1e400}` will have length 2. Finally, `float` called on a NaN will return the same object, so `x is float(x)` is `True`, and `{x, float(x)}` will have length 1.

### Rounding

The round() function rounding strategy and return type have
changed. Exact halfway cases are now rounded to the nearest even
result instead of away from zero. For the built-in types supporting
round(), values are rounded to the closest multiple of 10 to the power
minus n; if two multiples are equally close, rounding is done toward
the even choice.