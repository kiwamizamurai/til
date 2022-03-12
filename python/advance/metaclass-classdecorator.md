## Metaclass

At the last time,  we achieved

- [last til](https://github.com/kiwamizamurai/til/blob/main/python/signature.md)

```python
class Stock(Structure):
    __signature__ = make_signature(['name','shares','price'])
class Point(Structure):
    __signature__ = make_signature(['x', 'y'])
 ```

But, we can optimize them by using `decorator/metaclass`

```python
def add_signature(*names):
    def decorate(cls):
        cls.__signature__ = make_signature(names)
        return cls
    return decorate

@add_signature('name','shares','price')
class Stock(Structure):
    pass
```

In my opinison, MetaClass is better because


```python
class StructMeta(type):
    def __new__(cls, name, bases, clsdict):
        clsobj = super().__new__(cls, name, bases, clsdict)
        sig = make_signature(clsobj._fields)
        setattr(clsobj, '__signature__', sig)
        return clsobj

class Structure(metaclass=StructMeta):
    _fields = []
    def __init__(self, *args, **kwargs):
        bound = self.__signature__.bind(*args, **kwargs)
        for name, val in bound.arguments.items():
            setattr(self, name, val)

class Stock(Structure):
    _fields = ['name', 'shares', 'price']
```

However, he says

- Use a class decorator if the goal is to tweak
classes that might be unrelated
- Use a metaclass if you're trying to perform
actions in combination with inheritance



## Reference

- http://www.dabeaz.com/py3meta/Py3Meta.pdf