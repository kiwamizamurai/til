## multiple types


```py
from typing import Union

def bytes_getitem(b: bytes, idx: Union[int, slice]) -> Union[int, bytes]:
    # if isinstance(idx, int):
    #     return ...
    # else:
    #     return ...
    return b[idx]

foo = b'100'

print(reveal_type(bytes_getitem(foo, 0)))
print(bytes_getitem(foo, 0) - ord('0'))
print(reveal_type(bytes_getitem(foo, slice(1, None, None))))
```

run this, and got an error


```py
╰─❯ mypy a.py
a.py:19: note: Revealed type is 'Union[builtins.int, builtins.bytes]'
a.py:20: error: Unsupported operand types for - ("bytes" and "int")
a.py:20: note: Left operand is of type "Union[int, bytes]"
a.py:21: note: Revealed type is 'Union[builtins.int, builtins.bytes]'
Found 1 error in 1 file (checked 1 source file)
```

So we use `@overload`

```py
from typing import overload

@overload
def bytes_getitem(b: bytes, idx: int) -> int: ...

@overload
def bytes_getitem(b: bytes, idx: slice) -> bytes: ...


def bytes_getitem(b: bytes, idx: Union[int, slice]) -> Union[int, bytes]:
    return b[idx]
```


then we get no errors


```py
╰─❯ mypy a.py
a.py:19: note: Revealed type is 'builtins.int'
a.py:21: note: Revealed type is 'builtins.bytes'
```


## Reference

- https://sobolevn.me/2019/01/simple-dependent-types-in-python
- https://github.com/anthonywritescode/explains