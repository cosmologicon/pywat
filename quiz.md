# The Python wat quiz

Test your knowledge of Python edge cases!

In each snippet below, one or more values is missing (marked with `???`). In each case, answer Yes or No: is it possible to replace the `???` with something that will make the snippet into exactly what you would see if you entered the text into the interpreter?

We are looking for edge cases here, so you'll need to be careful, but I'm not trying to trick you, and I'm only trying to cover the basics. That means:

* Solutions, if they exist, are your basic, built-in values. `int`, `list`, `dict`, `str`, `None`, etc.
* Specifically, no user-defined classes, no `lambda`s, and nothing referencing `globals`, `locals`, etc.
* Nothing that changes value just by inspecting it, so no iterators or generators.
* No `float`s or unicode. Those things have enough edge cases in any language, and I just want to test Python-specific edge cases. This includes the float values `nan` and `inf`.
* Assume nothing has been `import`ed, and obviously no built-ins have been redefined.
* Assume the latest Python version (currently 3.5) if it matters. It shouldn't, though.

# Examples

## Example question 1

    >>> x = ???
    >>> x < x
    True

# Example answer 1

**No.** This snippet is impossible.

## Example question 2

    >>> x, y = ???
    >>> x + y == y + x
    False

## Example answer 2

**Yes.** This snippet is possible.

    >>> x, y = [0], [1]
    >>> x + y == y + x
    False

# Questions

Make sure to write down your answers (Yes or No) so you're not tempted to cheat.

For a *real* challenge, try taking the quiz without checking yourself with a Python interpreter. Good luck!

## Question 1: `max` vs slice

    >>> x, a, b, c = ???
    >>> max(x) < max(x[a:b:c])
    True

## Question 2: `min` of two elements

    >>> x, y = ???
    >>> min(x, y) == min(y, x)
    False

## Question 3: `any` vs addition

    >>> x, y = ???
    >>> any(x) and not any(x + y)
    True

## Question 4: `count` vs `len`

    >>> x, y = ???
    >>> x.count(y) <= len(x)
    False

## Question 5: Associative multiplication

    >>> x, y, z = ???
    >>> x * (y * z) == (x * y) * z
    False

## Question 6: `zip` vs comparison

    >>> x, y = ???
    >>> x < y and all(a >= b for a, b in zip(x, y))
    True

## Question 7: size of sets and lists

    >>> x = ???
    >>> len(set(list(x))) == len(list(set(x)))
    False

## Question 8: argument expansion

    >>> x = ???
    >>> min(x) == min(*x)
    False

## Question 9: zero `sum`

    >>> x, y = ???
    >>> sum(0 * x, y) == y
    False

## Question 10: `max` vs `in`

    >>> x, y = ???
    >>> y > max(x) and y in x
    True

# Answers

All done? Make sure you have your answers written down and check out the answers here.

