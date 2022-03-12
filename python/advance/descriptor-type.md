## Getter/Setter Descriptor

At the last time,  we achieved

- [last til](https://github.com/kiwamizamurai/til/blob/main/python/metaclass-classdecorator.md)

Still, we have a problem that is something like

```python
 s = Stock('ACME', 50, 91.1)
>>> s.name = 42
>>> s.shares = 'a heck of a lot'
>>> s.price = (23.45 + 2j)
```

We want to ensure the types of attributes

```python
class Stock(Structure):
    _fields = ['name', 'shares', 'price']
    @property
    def shares(self):
        return self._shares
    @shares.setter
    def shares(self, value):
        if not isinstance(value, int):
            raise TypeError('expected int')
        if value < 0:
            raise ValueError('Must be >= 0')
        self._shares = value
```

Then we can validate them

```python
>>> s = Stock('ACME', 50, 91.1)
>>> s.shares = 'a lot'
Traceback (most recent call last):
...
TypeError: expected int
>>> s.shares = -10
Traceback (most recent call last):
...
ValueError: Must be >= 0
>>> s.shares = 37
>>> s.shares
37
>>>
```

However, what if there are myriad of properties that have same logic?


The answer is `Descriptor Protocol`

```python
class Descriptor:
    def __get__(self, instance, cls):
        ...
    def __set__(self, instance, value):
        ...
    def __delete__(self, instance)
        ...
```

Properties are implemented via descriptors

here is a simple example

```python
class Spam:
    x = Descriptor()
s = Spam()
s.x          # x.__get__(s, Spam)
s.x = value  # x.__set__(s, value)
del s.x      # x.__delete__(s)
```


#### A Basic Descriptor

```python
class Descriptor:
    def __init__(self, name=None):
        self.name = name
    def __get__(self, instance, cls):
        if instance is None:
            return self
        else:
            return instance.__dict__[self.name]
    def __set__(self, instance, value):
        instance.__dict__[self.name] = value
    def __delete__(self, instance):
        del instance.__dict__[self.name]
```

You don't need __get__() if it merely returns the normal dictionary value


we can apply it for type check


```python
class Typed(Descriptor):
    ty = object
    def __set__(self, instance, value):
        if not isinstance(value, self.ty):
            raise TypeError('Expected %s' % self.ty)
        super().__set__(instance, value)
```

```python
class Integer(Typed):
    ty = int
class Float(Typed):
    ty = float
class String(Typed):
    ty = str
```


example usage

```python
class Stock(Structure):
    _fields = ['name', 'shares', 'price']
    name = String('name')
    shares = Integer('shares')
    price = Float('price')
```

```python
>>> s = Stock('ACME', 50, 91.1)
>>> s.shares = 'a lot'
Traceback (most recent call last):
...
TypeError: Expected <class 'int'>
>>> s.name = 42
Traceback (most recent call last):
...
TypeError: Expected <class 'str'>
>>>
```


To be continued

- [next til](https://github.com/kiwamizamurai/til/blob/main/python/mixin.md)