## Signatures binding

- [last til](https://github.com/kiwamizamurai/til/blob/main/python/init-needless.md)

A Signature object represents the call signature of a function and its return annotation. For each parameter accepted by the function it stores a Parameter object in its parameters collection.

```python
import inspect

def foo(a, b, x='blah'):
    pass

print(inspect.getfullargspec(foo))

FullArgSpec(args=['a', 'b', 'x'], varargs=None, varkw=None, defaults=('blah',), kwonlyargs=[], kwonlydefaults=None, annotations={})


print(inspect.signature(foo))

(a, b, x='blah')
```

Now, I am going to bind signature that was not acquired last time.

```python
import inspect
from inspect import Parameter, Signature

def make_signature(names):
    return Signature(
        Parameter(
            name,
            Parameter.POSITIONAL_OR_KEYWORD
        )
        for name in names
    )


class Structure:
    __signature__ = make_signature([])

    def __init__(self, *args, **kwargs):
        bound = self.__signature__.bind(
            *args, **kwargs
        )
        for name, val in bound.arguments.items():
            setattr(self, name, val)

class Stock(Structure):
    __signature__ = make_signature(
        ['name', 'shares', 'price']
    )
```

Now, we can get as follows

```python
s = Stock('ACME', shares=50, price=91.1)

print(inspect.signature(Stock))
(name, shares, price)
```

However, the thing is that this long coding is kind of hassle.

There are at least two solutions by the way

- Class decorators
- Metaclasses


- [next til](https://github.com/kiwamizamurai/til/blob/main/python/metaclass-classdecorator.md)