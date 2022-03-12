# First

`first` is an MIT-licensed Python package with a simple function that returns the first true value from an iterable, or None if there is none. If you need more power, you can also supply a key function that is used to judge the truth value of the element or a default value if None doesn’t fit your use case.

```python
from first import first

def greater_than_zero(number):
    return number > 0

first([-1, 0, 1, 2], key=greater_than_zero)
```