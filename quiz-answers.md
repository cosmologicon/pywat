# The Python wat quiz Answers

## STOP! SPOILER ALERT!

[Click here if you haven't taken the quiz.](https://github.com/cosmologicon/pywat/blob/master/quiz.md)

The quiz questions are repeated below along with their answers. At the very bottom there's a quick answer key if you just want to grade yourself quickly.

## Evaluate your score

Count the number of questions out of 10 that you had the correct answer for.

* 10/10: Python edge case gold medal!
* 9/10: Python edge case silver medal.
* 8/10: Python edge case bronze medal.
* 7/10 or less: This quiz is unable to distinguish your results from random chance with any statistical certainty. Sorry.

Just remember that edge cases can be interesting, but knowing the exact behavior of Python edge cases is not the same as being a good Python programmer. If you follow good programming practices, you shouldn't be encountering edge cases at all. The most important thing is to remember that edge cases exist when writing your tests!

## Questions with answers

### Question 1: `max` vs slice

No. This snippet is impossible.

    >>> x, a, b, c = ???
    >>> max(x) < max(x[a:b:c])
    True

### Question 2: `min` of two elements

Yes. This snippet is possible.

    >>> x, y = {0}, {1}
    >>> min(x, y) == min(y, x)
    False

### Question 3: `any` vs addition

No. This snippet is impossible.

    >>> x, y = ???
    >>> any(x) and not any(x + y)
    True

### Question 4: `count` vs `len`

Yes. This snippet is possible.

    >>> x, y = "a", ""
    >>> x.count(y) <= len(x)
    False

### Question 5: Associative multiplication

Yes. This snippet is possible.

    >>> x, y, z = [0], -1, -1
    >>> x * (y * z) == (x * y) * z
    False

### Question 6: `zip` vs comparison

Yes. This snippet is possible.

    >>> x, y = [], [0]
    >>> x < y and all(a >= b for a, b in zip(x, y))
    True

### Question 7: size of sets and lists

No. This snippet is impossible.

    >>> x = ???
    >>> len(set(list(x))) == len(list(set(x)))
    False

### Question 8: argument expansion

Yes. This snippet is possible.

    >>> x = [[0]]
    >>> min(x) == min(*x)
    False

### Question 9: zero `sum`

No. This snippet is impossible.

    >>> x, y = ???
    >>> sum(0 * x, y) == y
    False

### Question 10: `max` vs `in`

Yes. This snippet is possible.

    >>> x, y = "aa", "aa"
    >>> y > max(x) and y in x
    True

## Quick answer key

1. No
2. Yes
3. No
4. Yes
5. Yes
6. Yes
7. No
8. Yes
9. No
10. Yes
