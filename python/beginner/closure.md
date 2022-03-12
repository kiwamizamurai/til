
# Manipulate Closure

Closures can be used as technique for avoiding excessive code repetition.

```python
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age
    ...
    @property
    def age(self):
        return self._age

    @age.setter
    def age(self, value):
        if not isinstance(value, int):
            raise TypeError('Expected int')
        self._age = value
    ...
```

Instead of repeatedly typing that code over and over again, you can automatically create it using a closure.

```python
def typedproperty(name, expected_type):
    private_name = '_' + name
    @property
    def prop(self):
        return getattr(self, private_name)

    @prop.setter
    def prop(self, value):
        if not isinstance(value, expected_type):
            raise TypeError(f'Expected {expected_type}')
        setattr(self, private_name, value)

    return prop
```

Now, try it out by defining a class like this:

```python
class Person:
    name = typedproperty('name', str)
    age = typedproperty('age', int)

    def __init__(self, name, age):
        self.name = name
        self.age = age
```

Try creating an instance and verifying that type-checking works.

```python
p = Person('John', 50)

>>> s.name
'John'

>>> p.age = '100'
... TypeError: Expected <class 'int'> ...
```
