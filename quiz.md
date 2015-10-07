# The Python wat quiz

Test your knowledge of Python edge cases!

## Instructions

In each of the 10 snippets below, one or more values is missing (marked with `???`). In each case, answer Yes or No: is it possible to replace the `???` with something that will make the snippet into exactly what you would see if you entered the text into the interpreter?

For a *real* challenge, try taking the quiz without using a Python interpreter to check your answers.

## Examples

### Example question 1

    >>> x = ???
    >>> x < x
    True

### Example answer 1

**No.** This snippet is impossible.

### Example question 2

    >>> x, y = ???
    >>> x + y == y + x
    False

### Example answer 2

**Yes.** This snippet is possible.

    >>> x, y = [0], [1]
    >>> x + y == y + x
    False

## Details and scope

This quiz is designed to test edge cases, so be careful and consider all possibilities, but these are not trick questions. All answers are clearly within the scope of this quiz.

Python lets you do weird, tricky things, like using `exec` in an expression. So the answers to this quiz are limited in scope to the built-in types `bool`, `int`, `list`, `tuple`, `dict`, `set`, `frozenset`, `str`, and `NoneType`, and values of these types. That means:

* no user-defined classes
* no `lambda`s
* no use of `globals` or `locals`
* no `import`
* no redefining built-ins (obviously)
* nothing that changes value just by inspecting it or iterating over it (no iterators or generators)
* no `bytes` arrays, `buffer`s, `range`s, `memoryview`s, or dictionary views
* no `float`s or unicode, which have enough edge cases in any language. This also means no `nan` or `inf`. Strings, if they appear, will only have ascii characters.

Assume the latest Python version (currently 3.5) if it matters. It shouldn't, though.

## The questions

Make sure to write down your answers (Yes or No) *before* looking at the answers, so you're not tempted to cheat. Good luck!

### Question 1: `max` vs slice

    >>> x, a, b, c = ???
    >>> max(x) < max(x[a:b:c])
    True

### Question 2: `min` of two elements

    >>> x, y = ???
    >>> min(x, y) == min(y, x)
    False

### Question 3: `any` vs addition

    >>> x, y = ???
    >>> any(x) and not any(x + y)
    True

### Question 4: `count` vs `len`

    >>> x, y = ???
    >>> x.count(y) <= len(x)
    False

### Question 5: Associative multiplication

    >>> x, y, z = ???
    >>> x * (y * z) == (x * y) * z
    False

### Question 6: `zip` vs comparison

    >>> x, y = ???
    >>> x < y and all(a >= b for a, b in zip(x, y))
    True

### Question 7: size of sets and lists

    >>> x = ???
    >>> len(set(list(x))) == len(list(set(x)))
    False

### Question 8: argument expansion

    >>> x = ???
    >>> min(x) == min(*x)
    False

### Question 9: zero `sum`

    >>> x, y = ???
    >>> sum(0 * x, y) == y
    False

### Question 10: `max` vs `in`

    >>> x, y = ???
    >>> y > max(x) and y in x
    True

## Answers

All done? Make sure you have your answers written down and [check out the answers here](https://github.com/cosmologicon/pywat/blob/master/quiz-answers.md).

