## No need to wrtite init


```python
class Stock:
    def __init__(self, name, shares, price):
        self.name = name
        self.shares = shares
        self.price = price
class Point:
    def __init__(self, x, y):
        self.x = x
        self.y = y
```

create Structure

```python
class Structure:
    _fields = []
    def __init__(self, *args):
        if len(args) != len(self._fields):
            raise TypeError('Wrong # args')
        for name, val in zip(self._fields, args):
            setattr(self, name, val)

class Stock(Structure):
    _fields = ['name', 'shares', 'price']

class Point(Structure):
    _fields = ['x', 'y']
```


But this has problems

- No support for keyword args

```shell
Traceback (most recent call last):
  File "a.py", line 26, in <module>
    s = Stock('ACME', price=123.45, shares=50)
TypeError: __init__() got an unexpected keyword argument 'price'
```

- Missing calling signatures
```shell
(*args)
```

desired one is

```shell
(name, shares, price)
```

Wanna solution? please check next `til`

- [next til](https://github.com/kiwamizamurai/til/blob/main/python/signature.md)


## References

- http://www.dabeaz.com/py3meta/Py3Meta.pdf