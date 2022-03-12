## Size check

At the last time,  we achieved

- [last til](https://github.com/kiwamizamurai/til/blob/main/python/mixin.md)

we can manipulate the descriptor like this,

```python
class Sized(Descriptor):
    def __init__(self, *args, maxlen, **kwargs):
        self.maxlen = maxlen
        super().__init__(*args, **kwargs)
    def __set__(self, instance, value):
        if len(value) > self.maxlen:
            raise ValueError('Too big')
        super().__set__(instance, value)
```

use it as mixin as same as the last time, and with `maxlen` argument this time


```python
class SizedString(String, Sized):
     pass

class Stock(Structure):
    _fields = ['name', 'shares', 'price']
    name = SizedString('name', maxlen=8)
    shares = PosInteger('shares')
    price = PosFloat('price')
```

example

```python
>>> s = Stock('ACME', 50, 91.1)
>>> s.name = 'ABRACADABRA'
Traceback (most recent call last):
...
ValueError: Too big
>>>
```