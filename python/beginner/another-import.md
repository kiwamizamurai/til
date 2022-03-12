# Another way to import

the import keyword is actually a wrapper around a function named __import__.

```python
import sys
import os
from pprint import pprint
cl = __import__('COLLECTIONS'.lower())


print(cl)
pprint(sys.modules['collections'])
print(sys.builtin_module_names)
```

Check

```python
<module 'collections' from '/usr/local/var/pyenv/versions/3.6.8/lib/python3.6/collections/__init__.py'>
<module 'collections' from '/usr/local/var/pyenv/versions/3.6.8/lib/python3.6/collections/__init__.py'>
('_ast', '_codecs', '_collections', '_functools', '_imp', '_io', '_locale', '_operator', '_signal', '_sre', '_stat', '_string', '_symtable', '_thread', '_tracemalloc', '_warnings', '_weakref', 'atexit', 'builtins', 'errno', 'faulthandler', 'gc', 'itertools', 'marshal', 'posix', 'pwd', 'sys', 'time', 'xxsubtype', 'zipimport')
```