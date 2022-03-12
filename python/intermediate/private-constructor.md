# Private Constructor

this keeps the class publicly visible (allowing isinstance etc.) but ensures construction is only possible by class-methods:

```python
class OnlyCreatable(object):

    __create_key = object()

    @classmethod
    def create(cls, value):
        return OnlyCreatable(cls.__create_key, value)

    def __init__(self, create_key, value):
        assert(create_key == OnlyCreatable.__create_key), \
            "OnlyCreatable objects must be created using OnlyCreatable.create"
        self.value = value
```

Constructing an object with the `create class-method`:

```python
OnlyCreatable.create("I'm a test")
<__main__.OnlyCreatable at 0x7f0dfa3e3278>
```

When attempting to construct an object without using the `create class-method` creation fails due to the assertion:

```python
OnlyCreatable(0, "I'm a test")

AssertionError: OnlyCreatable objects must be created using OnlyCreatable.create
```

If attempting to create an object by mimicking the `create class-method` creation fails due to compiler mangling of `OnlyCreatable.__createKey`.

```python
OnlyCreatable(OnlyCreatable.__createKey, "I'm a test")
AttributeError: type object 'OnlyCreatable' has no attribute '__createKey'
```

The only way to construct `OnlyCreatable` outside of a class-method is to know the value of `OnlyCreatable.__create_key`. Since this class-attribute's value is generated at runtime and it's name is prefixed with \_\_ marking it as inaccessible it is effectively 'impossible' to obtain this value and/or construct the object.

- https://stackoverflow.com/questions/8212053/private-constructor-in-python
- https://stackoverflow.com/questions/920045/what-is-the-need-of-private-constructor-in-c
