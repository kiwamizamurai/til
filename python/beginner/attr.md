## attr module


```py
import attr

@attr.s(auto_attribs=True)
class Book(object):
    isbn: str
    name: str
    author: str
    published_year: int
    edition: int

print(Book())
```

```py
∃! python b.py
Traceback (most recent call last):
  File "b.py", line 11, in <module>
    print(Book())
TypeError: __init__() missing 5 required positional arguments: 'isbn', 'name', 'author', 'published_year', and 'edition'
```


Also you can do validation for property

```py
import attr
import re

@attr.s(auto_attribs=True)
class Book(object):
    isbn: str = attr.ib()
    @isbn.validator
    def pattern_match(self, attribute, value):
        m = re.match(r"^(\d{3}-)\d{1,3}-\d{2,3}-\d{1,7}-\d$", value)
        if not m:
            raise ValueError("incorrect format for isbn", value)
    name: str
    author: str
    published_year: int
    edition: int

print(Book(isbn="isbn", name="name", author="author", published_year=2, edition=3))
```

```py
∃! python b.py
Traceback (most recent call last):
  File "b.py", line 17, in <module>
    print(Book(isbn="isbn", name="name", author="author", published_year=2, edition=3))
  File "<attrs generated init __main__.Book>", line 8, in __init__
  File "b.py", line 11, in pattern_match
    raise ValueError("incorrect format for isbn", value)
ValueError: ('incorrect format for isbn', 'isbn')
```


`repr` is automatically generated

```py
print(Book(isbn="978-3-16-148410-0", name="name", author="author", published_year=2, edition=3))

∃! python b.py
Book(isbn='978-3-16-148410-0', name='name', author='author', published_year=2, edition=3)
```


## Reference

- https://opensource.com/article/19/5/python-attrs
- https://stackoverflow.com/questions/54570103/how-to-use-attr-validators-optional