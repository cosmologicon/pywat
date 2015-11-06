# The Python wat quiz Answers

## STOP! SPOILER ALERT!

[Click here if you haven't taken the quiz.](https://github.com/cosmologicon/pywat/blob/master/quiz.md)

The quiz questions are repeated below along with their answers. At the very bottom there's a quick answer key if you just want to grade yourself quickly.

## Evaluate your score

Count the number of questions out of 10 that you had the correct answer for.

* 12/12: Congratulations! You are an expert at Python edge cases!
* 11/12: Great job! You are proficient at Python edge cases!
* 10/12: Not bad! You are skilled at Python edge cases!
* 9/12 or less: This quiz is unable to distinguish your results from random chance with any statistical certainty. Sorry.

Just remember that edge cases can be interesting, but knowing the exact behavior of Python edge cases is not the same as being a good Python programmer. If you follow good programming practices, you can avoid problems caused by edge cases, even ones you're not aware of.

I hope this was fun, though. Thanks for trying it out!

## Questions with answers

### Question 1: `min` of two elements

**Yes.** This snippet is possible.

```python
>>> x, y = {0}, {1}
>>> min(x, y) == min(y, x)
False
```

### Question 2: size of sets and lists

**No.** This snippet is impossible.

```python
>>> x = ???
>>> len(set(list(x))) == len(list(set(x)))
False
```


### Question 3: `type` vs `map`

**Yes.** This snippet is possible.

```python
>>> x, s = True, {1}
>>> s.add(x)
>>> type(x) in map(type, s)
False
```

### Question 4: `zip` vs comparison

**Yes.** This snippet is possible.

```python
>>> x, y = [], [0]
>>> x < y and all(a >= b for a, b in zip(x, y))
True
```

### Question 5: zero `sum`

**No.** This snippet is impossible.

```python
>>> x, y = ???
>>> sum(0 * x, y) == y
False
```

### Question 6: argument expansion

**Yes.** This snippet is possible.

```python
>>> x = [[0]]
>>> min(x) == min(*x)
False
```

### Question 7: Associative multiplication

**Yes.** This snippet is possible.

```python
>>> x, y, z = [0], -1, -1
>>> x * (y * z) == (x * y) * z
False
```

### Question 8: `max` vs `in`

**Yes.** This snippet is possible.

```python
>>> x, y = "aa", "aa"
>>> y > max(x) and y in x
True
```

### Question 9: `any` vs addition

**No.** This snippet is impossible.

```python
>>> x, y = ???
>>> any(x) and not any(x + y)
True
```

### Question 10: `count` vs `len`

**Yes.** This snippet is possible.

```python
>>> x, y = "a", ""
>>> x.count(y) <= len(x)
False
```

### Question 11: `all` vs `filter`

**No.** This snippet is impossible.

```python
>>> x = ???
>>> all(filter(None, x))
False
```

### Question 12: `max` vs slice

**No.** This snippet is impossible.

```python
>>> x, a, b, c = ???
>>> max(x) < max(x[a:b:c])
True
```

## Quick answer key

1. Yes
2. No
3. Yes
4. Yes
5. No
6. Yes
7. Yes
8. Yes
9. No
10. Yes
11. No
12. No

