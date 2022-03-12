
## Run HelloWorld with ast module

```python
import ast

hello_world = ast.Str(s='hello world!', lineno=1, col_offset=1)
print(hello_world)

print_name = ast.Name(id='print', ctx=ast.Load(), lineno=1, col_offset=1)
print(print_name)

print_call = ast.Call(func=print_name, ctx=ast.Load(), args=[hello_world], keywords=[], lineno=1, col_offset=1)
print(print_call)

module = ast.Module(body=[ast.Expr(print_call, lineno=1, col_offset=1)], lineno=1, col_offset=1)
print(module)

code = compile(module, '', 'exec')
print(code)
```

output is as follows:

```
<_ast.Str object at 0x7f16394d7f60>
<_ast.Name object at 0x7f16394d1048>
<_ast.Call object at 0x7f16394d10b8>
<_ast.Module object at 0x7f16394d1128>
<code object <module> at 0x7f1639d66ed0, file "", line 1>
```

Run with `eval`

```python
    eval(code)
```

we got

```python
    hello world!
```