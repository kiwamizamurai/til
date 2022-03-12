
# How private varibales act themself

Each output is in docstring


First, create `test2.py`

```python
def function():
    return 'function'

def _function():
    return '_function'
```

Import it with wildcard, and use it

`from test2 import *`

```python
class Test:
    """variables
    >>> t.foo
    11
    >>> t._bar
    23
    >>> t.external_func()
    'external method'
    >>> t._internal_func()
    '_internal method'
    >>> t.runFunction()
    'function'
    >>> t.runUnderscoreFunction()
    Traceback (most recent call last):
        ...
    NameError: name '_function' is not defined
    >>> t.use_class_as_name('someClass_')
    1
    """
    def __init__(self):
        self.foo = 11
        self._bar = 23
        self.__baz = 32

    def external_func(self):
        return 'external method'

    def _internal_func(self):
        return '_internal method'

    def runFunction(self):
        return function()

    def runUnderscoreFunction(self):
        # Now if you use a wildcard import to import all names from the module,
        # Python will not import names with a leading underscore
        # (unless the module defines an __all__ list that overrides this behavior)
        # Unlike wildcard imports, regular imports are not affected by the leading single underscore naming convention
        return _function()

    def use_class_as_name(self, class_):
        # Sometimes the most fitting name for a variable is already taken by a keyword.
        # you can not do like this: use_class_as_name(self, class):
        # a single trailing underscore (postfix) is used by convention
        # to avoid naming conflicts with Python keywords
    return 1
```

Run it

```python
if __name__ == '__main__':
    import doctest
    doctest.testmod(
        extraglobs={
            't': Test(),
        },
        verbose=False
    )
```

Next, check inheritance

```python
class ExtendedTest(Test):
    """variables
    >>> e.foo
    'overridden'
    >>> e._bar
    'overridden'
    >>> e.__baz
    Traceback (most recent call last):
        ...
    AttributeError: 'ExtendedTest' object has no attribute '__baz'
    >>> e._ExtendedTest__baz
    'overridden'
    >>> e._Test__baz
    32
    >>> e.get_mangled()
    'overridden'
    """
    def __init__(self):
        super().__init__()
        self.foo = 'overridden'
        self._bar = 'overridden'
        self.__baz = 'overridden'

def get_mangled(self):
    return self.__baz
```

Run it

```python
if __name__ == '__main__':
    import doctest
    doctest.testmod(
        extraglobs={
            't': Test(),
            'e': ExtendedTest()
        },
        verbose=False
    )
```

## Refefence

-  https://stackoverflow.com/questions/12592/can-you-check-that-an-exception-is-thrown-with-doctest-in-python