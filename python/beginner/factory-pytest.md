# Factory pytest fixture

Create function and object which is going to be tested

```python
    import pytest

    class Person(object):
        def __init__(self, name, favorite_animal=None):
            self.name = name
            self.favorite_animal = favorite_animal

    def anyone_like_dogs(people):
        return any(
            [p.favorite_animal == "dog" for p in people]
        )
```

Fixtures are functions, which will run before each test function to which it is applied. Fixtures are used to feed some data to the tests such as database connections, URLs to test and some sort of input data.

```python
@pytest.fixture()
def person(**kwargs):
    count = 0

    def _person(**kwargs):
        nonlocal count
        count += 1
        # create unique name whenever arguments does not have name
        name = kwargs.pop("name", "Bob {}".format(count))
        return Person(name=name, **kwargs)

    return _person
```


Write some tests

```python
def test_anyone_like_dogs_true(person):
    people = [
        person(favorite_animal="cat"),
        person(favorite_animal="dog")
    ]
    assert anyone_like_dogs(people) is True

def test_person(person):
    assert person().name != person().name
    assert person(name="Alice").name == "Alice"
```

Run it

```python
$ pipenv run pytest --capture=no --verbose test_example.py
===================================================================== test session starts ======================================================================
platform darwin -- Python 3.8.4, pytest-6.0.1, py-1.9.0, pluggy-0.13.1 -- /.venv/bin/python
cachedir: .pytest_cache
rootdir: /project
collected 2 items

test_example.py::test_anyone_like_dogs_true PASSED
test_example.py::test_person PASSED

====================================================================== 2 passed in 0.01s =======================================================================
```