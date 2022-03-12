
## Mixin

**Mixin** is quite difficult to understand. Here are some ideas that I found on the Internet.

- You want to provide a lot of optional features for a class.
- You want to use one particular feature in a lot of different classes.
- mixins are basically abstract classes without any internal states.
- mixin is a class that contains methods for use by other classes without having to be the parent class of those other classes.


At the last time,  we achieved

- [last til](https://github.com/kiwamizamurai/til/blob/main/python/descriptor-type.md)


```python
class Positive(Descriptor):
    def __set__(self, instance, value):
        if value < 0:
            raise ValueError('Expected >= 0')
        super().__set__(instance, value)


class PosInteger(Integer, Positive):
    pass
class PosFloat(Float, Positive):
    pass


class Stock(Structure):
    _fields = ['name', 'shares', 'price']
    name = String('name')
    shares = PosInteger('shares')
    price = PosFloat('price')
```


usage

```python
>>> s = Stock('ACME', 50, 91.1)
>>> s.shares = -10
Traceback (most recent call last):
...
ValueError: Expected >= 0
>>> s.shares = 'a lot'
Traceback (most recent call last):
...
TypeError: Expected <class 'int'>
>>>
```




## Reference

- https://rednafi.github.io/digressions/python/2020/07/03/python-mixins.html#mixins
- https://stackoverflow.com/questions/533631/what-is-a-mixin-and-why-are-they-useful
- https://en.wikipedia.org/wiki/Mixin
- https://www.thedigitalcatonline.com/blog/2020/03/27/mixin-classes-in-python/
- https://stackoverflow.com/questions/860245/mixin-vs-inheritance