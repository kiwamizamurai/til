# Mocking a function within a class method

app/utils.py

```python
def func():
    ...
    return resp  # outcome is a HTTPResponse object
```

app/models.py

```python
from app.utils import func

class MyModel(models.Model):

    # fields

    def call_func(self):
        ...
        func()
        ...

fake_response = mock.MagicMock(
    headers={'content-type': 'text/html'},
    status_code=2000,
    content="Fake 200 Response"
)
```

One way

```python
class MyModelTestCase(TestCase):

    fixtures = ['my_model_fixtures.json']

    def setUp(self):
        my_model = MyModel.objects.get(id=1)

    @mock.patch('app.models.func', return_value=fake_response)
    def test_my_model(self, fake_response):  # the mock goes in as a param or else you get number of arguments error!
        my_model.call_func()
        self.assertTrue(fake_response.called)
```

Other way

```python
def mock_func():
    def fake_func(*args,**kwargs):
        return 0
    m = mock.Mock()
    m.side_effect = fake_func
    return m
```

test_func.py

```python
@mock.patch(
    'app.models.func', # where the particular functions is called
    mock_func()
)
def test_function():
    ...
```

## Reference

- https://stackoverflow.com/questions/29458327/mocking-a-function-within-a-class-method
