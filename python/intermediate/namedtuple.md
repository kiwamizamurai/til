
# NamedTuple



```python
from typing import NamedTuple

def _make_named_attr_gettr(idx: int, name: str):
    def getter(self):
        return self[idx]
    getter.__name__ = name
    return property(getter)

def _namedtuple_repr(self) -> str:
    fields_s = ', '.join(
        f'{name}={getattr(self, name)!r}'
        for name in self._fields
    )
    return f'{type(self).__name__}({fields_s})'

def make_namedtuple(clsname, fields):
    attrs = {'_fields': fields, '__repr__': _namedtuple_repr}

    new_method_s = f'''\
def __new__(cls, {", ".join(fields)}):
    return tuple.__new__(cls, ({", ".join(fields)}))
'''
    exec(new_method_s, attrs)

    for idx, name in enumerate(fields):
        attrs[name] = _make_named_attr_gettr(idx, name)

    return type(clsname, (tuple,), attrs)

class User(make_namedtuple('User', ('name', 'age', 'location'))):
    __slots__ = ()

    def print_me(self):
        print(f'{self.name} is {self.age} years old and from {self.location}')

def get_users():
    return [
        User('John', 22, 'JAPAN'),
        User('X', 33, 'JAMAICA'),
        User('aaaaa', 2, 'USA')
    ]

for user in get_users():
    user.print_me()
```

this is equivalent to

```python
class User(tuple):
    _fields = ('name', 'age', 'location')

    def __new__(cls, name, age, location):
        return tuple.__new__(cls, (name, age, location))

    @property
    def name(self):
        return self[0]

    @property
    def age(self):
        return self[1]

    @property
    def location(self):
        return self[2]

    def __repr__(self):
        return (
            f'{type(self).__name__}('
            f'name={self.name}, age={self.age}, location={self.location},'
            f')'
        )
```