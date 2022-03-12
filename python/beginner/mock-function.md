
# Mock

In unittest, we are likely to do unittest

```python
from unittest import mock
import pytest
import requests

class MyError(Exception):
    pass

def do_something():
    try:
        r = requests.get("http://www.example.com/")
    except IOError:
        pass
    else:
        if r.status_code == 200:
            return 'Example Domain' in r.content
    raise MyError("Something bad happened")

def get_fake_get(status_code, content):
    m = mock.Mock()
    m.status_code = status_code
    m.content = content
    def fake_get(url):
        return m
    return fake_get

def raise_get(url):
    raise IOError("Unable to fetch url %s" % url)

@mock.patch('requests.get', get_fake_get(200, 'Example Domain です'))
def test_content():
    assert do_something() is True

@mock.patch('requests.get', get_fake_get(200, 'Exampleeeee Domain'))
def test_bad_content():
    assert do_something() is False

@mock.patch('requests.get', get_fake_get(404, 'Whatever'))
def test_bad_status_code():
    with pytest.raises(MyError):
        do_something()

@mock.patch('requests.get', raise_get)
def test_ioerror():
    with pytest.raises(MyError):
        do_something()
```

Run it

```python
$ pipenv run pytest --capture=no --verbose test_sample.py
============================================================ test session starts =============================================================
platform darwin -- Python 3.8.4, pytest-6.0.1, py-1.9.0, pluggy-0.13.1 -- /.venv/bin/python
cachedir: .pytest_cache
rootdir: /project, configfile: pytest.ini
collected 4 items

test_sample.py::test_content PASSED
test_sample.py::test_bad_content PASSED
test_sample.py::test_bad_status_code PASSED
test_sample.py::test_ioerror PASSED

============================================================= 4 passed in 0.14s ==============================================================
```