
# min/max implementation with functools

use `import functools`

```python
def reduce(function, iterable, initializer=None):
    it = iter(iterable)
    if initializer is None:
        value = next(it)
    else:
        value = initializer
    for element in it:
        value = function(value, element)
    return value

def min_reduce(iterable):
    return functools.reduce(lambda x, y: x if x < y else y, iterable)


def max_reduce(iterable):
    return functools.reduce(lambda x, y: x if x > y else y, iterable)
```

Check these outputs


```python
print(min_reduce([4, 6, 6, 78]))
4
print(max_reduce([4, 6, 6, 78]))
78
```