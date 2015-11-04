# The Python wat quiz

Test your knowledge of Python edge cases!

## Instructions

In each of the 10 snippets below, one or more values is missing (marked with `???`). In each case, answer Yes or No: is it possible to replace the `???` with something that will make the snippet into exactly what you would see if you entered the text into the interpreter?

For a *real* challenge, try taking the quiz without using a Python interpreter to check your answers.

## Examples

### Example question 1

```python
>>> x = ???
>>> x < x
True
```

### Example answer 1

**No.** This snippet is impossible.

### Example question 2

```python
>>> x, y = ???
>>> x + y == y + x
False
```

### Example answer 2

**Yes.** This snippet is possible.

```python
>>> x, y = [0], [1]
>>> x + y == y + x
False
```

## Details and scope

**The missing values in this quiz are limited to the built-in types `bool`, `int`, `list`, `tuple`, `dict`, `set`, `frozenset`, `str`, and `NoneType`, and values of these types.**

In particular, the following are out of scope, and are not valid as missing values:

* user-defined classes
* `lambda`s
* anything using `globals` or `locals`
* anything using `import`
* anything that changes value just by inspecting it or iterating over it (iterators and generators)
* expressions with side effects (e.g. `eval`), including anything that redefines built-ins (obviously)
* `bytes` arrays, `buffer`s, `range`s, `memoryview`s, and dictionary views
* `float`s and unicode, which have enough edge cases in any language. This means that `nan` and `inf` are also out of scope. Strings, if they appear, only have ascii characters.

These are not trick questions. For snippets whose answers are *Yes, this is possible*, the missing values will clearly be in scope.

Assume the latest Python version (currently 3.5) if it matters. It shouldn't, though.

## The questions

Make sure to write down your answers (Yes or No) *before* looking at the answers, so you're not tempted to cheat. Good luck!

### Question 1: `max` vs slice

```python
>>> x, a, b, c = ???
>>> max(x) < max(x[a:b:c])
True
```

### Question 2: `min` of two elements

```python
>>> x, y = ???
>>> min(x, y) == min(y, x)
False
```

### Question 3: `any` vs addition

```python
>>> x, y = ???
>>> any(x) and not any(x + y)
True
```

### Question 4: `count` vs `len`

```python
>>> x, y = ???
>>> x.count(y) <= len(x)
False
```

### Question 5: Associative multiplication

```python
>>> x, y, z = ???
>>> x * (y * z) == (x * y) * z
False
```

### Question 6: `zip` vs comparison

```python
>>> x, y = ???
>>> x < y and all(a >= b for a, b in zip(x, y))
True
```

### Question 7: size of sets and lists

```python
>>> x = ???
>>> len(set(list(x))) == len(list(set(x)))
False
```

### Question 8: argument expansion

```python
>>> x = ???
>>> min(x) == min(*x)
False
```

### Question 9: zero `sum`

```python
>>> x, y = ???
>>> sum(0 * x, y) == y
False
```

### Question 10: `max` vs `in`

```python
>>> x, y = ???
>>> y > max(x) and y in x
True
```

## Answers

All done? Make sure you have your answers written down and [check out the answers here](https://github.com/cosmologicon/pywat/blob/master/quiz-answers.md).

