## How to group a decorator


```python
def ratelimit(params):
    def _ratelimit(f):
        def wrapper(self, *args, **kwargs):
            period_remaining = _period_remaining()
            if period_remaining <= 0:
                params.calls = 0
                params.last_reset = params.clock()
            params.calls += 1
            if params.calls > params.max_calls:
                if params.raise_exception:
                    raise Exception('too many calls', _period_remaining())
                return
            return f(self, *args, **kwargs)
        return wrapper

    def _period_remaining():
        elapsed = params.clock() - params.last_reset
        return params.period - elapsed
    return _ratelimit

class Field:
    def __init__(self) -> None:
        self.value = None
    def __set_name__(self, owner, name):
        self.internal_name = f'_{self.__class__}_{name}'
    def __get__(self, instance, instance_type):
        if instance is None:
            return self
        return getattr(instance, self.internal_name, None)

class Tier(Field):
    def __init__(self, max_calls=5, period=60, clock=time.monotonic, raise_exception=True):
        super().__init__()
        self.calls = 0
        self.max_calls = max_calls
        self.period = period
        self.clock = clock
        self.raise_exception = raise_exception
        self.last_reset = clock()
```


example usage


```python
class T:
    tier1 = Tier(max_calls=5)

    @ratelimit(tier1)
    def bb(self):
        print("bb called")

    @ratelimit(tier1)
    def cc(self):
        print("cc called")


for _ in range(3):
    t.bb()
    t.cc()
```


## Reference

- https://stackoverflow.com/questions/11731136/class-method-decorator-with-self-arguments
- https://stackoverflow.com/questions/2035423/python-how-to-refer-to-the-class-from-within-it-like-the-recursive-function
- https://stackoverflow.com/questions/12078288/scope-of-variables-in-python-decorators-changing-parameters