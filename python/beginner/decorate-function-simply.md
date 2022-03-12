
# Create simple decorator 

First, import some libraries:

```python
from functools import wraps
import os
import logging

logging.basicConfig(level=logging.DEBUG)
os.environ["DEBUG"]
```

Create a decorator which yield log

```python
def debug(func):
  logger = logging.getLogger(__name__)
  if 'DEBUG' not in os.environ:
    return func
  msg = func.__qualname__ 
  @wraps(func)
  def wrapper(*args, **kwargs):
    logger.debug(msg)
    return func(*args, **kwargs)
  return wrapper
```

Run this:

```python
@debug
def doSomething():
  print('I am working hard')
```

Output is as follows:

```python
    doSomething()
    DEBUG:__main__:doSomething
    I am working hard
```