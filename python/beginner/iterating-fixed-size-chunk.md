## How to iterate in fixed-size chunks


```py
import itertools


def chunked_iterable(iterable, size):
    it = iter(iterable)
    while True:
        chunk = tuple(itertools.islice(it, size))
        if not chunk:
            print("end")
            break
        yield chunk


for chunk in chunked_iterable([1, 2, 3, 4, 5, 6, 7, 8, 9, 10], 3):
    print(chunk)
```

output

```python
∃! python a.py
(1, 2, 3)
(4, 5, 6)
(7, 8, 9)
(10,)
end
```


## References

- https://alexwlchan.net/2018/12/iterating-in-fixed-size-chunks/