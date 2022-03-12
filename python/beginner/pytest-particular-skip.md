# Pytest Skip and run specific

The pytest.mark.skip decorator skips the decorated test function unconditionally,
so you’ll use it when a test always needs to be skipped.

```python
    import pytest
    try:
        import mylib
    except ImportError:
        mylib = None

    @pytest.mark.skip("Do not run this")
    def test_fail():
        assert False

    @pytest.mark.skipif(mylib is None, reason="mylib is not available")
    def test_mylib():
        assert mylib.foobar() == 42

    def test_skip_at_runtime():
        if True:
            pytest.skip("Finally I don't want to run it")
```

Run it

```python
    $ pipenv run pytest --capture=no --verbose test_sample.py
    =========================================================== test session starts ============================================================
    platform darwin -- Python 3.8.4, pytest-6.0.1, py-1.9.0, pluggy-0.13.1 -- /.venv/bin/python
    cachedir: .pytest_cache
    rootdir: /project
    collected 2 items

    test_sample.py::test_fail SKIPPED
    test_sample.py::test_mylib SKIPPED

    ============================================================ 2 skipped in 0.01s ============================================================
```

Pytest provides a dynamic marking system that allows you to mark tests with a key- word that can be used as a filter. To mark tests in this way, use the -m option.

```python
    @pytest.mark.mytest
    def test_something():
        a = ['a', 'b']
        assert a == a
```

Prepare `pytest.ini`

```yaml
    [pytest]
    markers =
        mytest: mark a test as a mytest.
```

Run it

```python
    $ pipenv run pytest --capture=no --verbose test_sample.py -m mytest
    =========================================================== test session starts ============================================================
    platform darwin -- Python 3.8.4, pytest-6.0.1, py-1.9.0, pluggy-0.13.1 -- /.venv/bin/python
    cachedir: .pytest_cache
    rootdir: /project, configfile: pytest.ini
    collected 4 items / 3 deselected / 1 selected

    test_sample.py::test_something PASSED

    ===================================================== 1 passed, 3 deselected in 0.01s ======================================================
```

### References

- https://docs.pytest.org/en/latest/example/markers.html#registering-markers