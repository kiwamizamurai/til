
# One way to use Ast for detect test files

This is just an example to utilize `ast`

Create `test_ast.py`

```python
class Test(unittest.TestCase):
    def test_hello(self):
        pass
```

Create `find_test.py`

```python
import glob
import os
import ast

def is_test_file(abspath) -> bool:
    print(abspath)
    with open(abspath, "r") as f:
        data = f.read()
    source_ast = ast.parse(data)
    for node in source_ast.body:
        if isinstance(node, ast.ClassDef) and _is_testcase_class(node):
            for class_node in node.body:
                if isinstance(class_node, ast.FunctionDef) and class_node.name.startswith("test_"):
                    return True
                return False
        return False
    return False

def _is_testcase_class(classdef: ast.ClassDef) -> bool:
    for base_class in classdef.bases:
        # if the test class looks like class Test(TestCase)
        if isinstance(base_class, ast.Name):
            base_name = base_class.id

        # if the test class looks like class Test(unittest.TestCase):
        elif isinstance(base_class, ast.Attribute):
            base_name = base_class.attr
        else:
            continue
        if base_name == 'TestCase':
            return True
        return False

for path in glob.glob('./*.py'):
    print(is_test_file(os.path.abspath(path)))
```

The output is as follows:

```python
    $ pipenv run python find_test.py
    Loading .env environment variables…
    /path/to/project/find_test.py
    False
    /path/to/project/test_ast.py
    True
```

## References

- https://vpyast.appspot.com/
- https://rotemtam.com/2020/08/13/python-ast/