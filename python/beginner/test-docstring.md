
# Run test with Docstring

`doctest` allows us to test code inside docsting.

```python
    class Example(object):

        def add(self, a, b):
            '''
            >>> e.add(10, 10)
            20
            >>> e.add(1, 10)
            2
            '''
            return a + b

        def sub(self, a, b):
            '''
            >>> e.sub(5, 10)
            -5
            >>> e.sub(1, 10)
            9
            '''
            return a - b

    if __name__ == '__main__':
        import doctest
        doctest.testmod(extraglobs={'e': Example()}, verbose=True)
```

Output is as follows:

```python
    Trying:
        e.add(10, 10)
    Expecting:
        20
    ok
    Trying:
        e.add(1, 10)
    Expecting:
        2
    **********************************************************************
    File "test.py", line 7, in __main__.Example.add
    Failed example:
        e.add(1, 10)
    Expected:
        2
    Got:
        11
    Trying:
        e.sub(5, 10)
    Expecting:
        -5
    ok
    Trying:
        e.sub(1, 10)
    Expecting:
        9
    **********************************************************************
    File "test.py", line 16, in __main__.Example.sub
    Failed example:
        e.sub(1, 10)
    Expected:
        9
    Got:
        -9
    2 items had no tests:
        __main__
        __main__.Example
    **********************************************************************
    2 items had failures:
    1 of   2 in __main__.Example.add
    1 of   2 in __main__.Example.sub
    4 tests in 4 items.
    2 passed and 2 failed.
    ***Test Failed*** 2 failures.
```