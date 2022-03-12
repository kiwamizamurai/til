
## Process After init


```python
import dataclasses


@dataclasses.dataclass
class A:
    a: int = 55

    def __post_init__(self):
        print("parent __post_init__ got called", self)

@dataclasses.dataclass
class B(A):
    b: str = 'b'

    def __post_init__(self):
        print("child __post_init__ got called", self)
        super().__post_init__()

a = A(33)

b = B()
```

output

```python
parent __post_init__ got called A(a=33)
child __post_init__ got called B(a=55, b='b')
parent __post_init__ got called B(a=55, b='b')
```

However, a private variable has a issue

```python
from dataclasses import dataclass


@dataclass
class Vehicle:
    _wheels: int

    @property
    def wheels(self) -> int:
        print("getting wheels")
        return self._wheels

    @wheels.setter
    def wheels(self, wheels: int):
        print("setting wheels to", wheels)
        self._wheels = wheels
```

the problem is

```python
Traceback (most recent call last):
  File "a.py", line 20, in <module>
    car = Vehicle(wheels=4)
TypeError: __init__() got an unexpected keyword argument 'wheels'
```

### IntVar

solution is to use `IntVar`

```python
from dataclasses import dataclass, InitVar


@dataclass
class Vehicle:

    wheels: InitVar[int]
    _wheels: int = None  # default given => not required in __init__()

    def __post_init__(self, wheels: int):
        self._wheels = wheels
```

Granted, __init__() now expects a wheels argument instead of _wheels, which is what we want.


## Reference

- https://florimond.dev/blog/articles/2018/10/reconciling-dataclasses-and-properties-in-python/