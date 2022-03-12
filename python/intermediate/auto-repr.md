## Auto repr

class decorator

```python
def auto_repr(cls):
    def __repr__(self):
        items = ("%s=%r" % (k, v) for k, v in self.__dict__.items())
        return "%s(%s)" % (self.__class__.__name__, ', '.join(items))
    cls.__repr__ = __repr__
    return cls
```

```python
@auto_str
class Foo(object):
    def __init__(self, value_1, value_2):
        self.attribute_1 = value_1
        self.attribute_2 = value_2
```

this will cause a problem when you are using sqlalchemy

in that case, use this

```python
def auto_repr(cls):
    def __repr__(self):

        excl = ('_sa_adapter', '_sa_instance_state')
        field_dict = {
            k: v for k, v in vars(self).items()
            if not k.startswith('_') and not any(hasattr(v, a) for a in excl)}
        params = ', '.join("%s=%r" % (k, v) for k, v in field_dict.items())
        return f"{self.__class__.__name__}({params})"
    cls.__repr__ = __repr__
    return cls
```

- https://stackoverflow.com/questions/55713664/sqlalchemy-best-way-to-define-repr-for-large-tables
- https://stackoverflow.com/questions/43042044/sqlalchemy-table-schema-autoload
- https://stackoverflow.com/questions/54026174/proper-autogenerate-of-str-implementation-also-for-sqlalchemy-classes
- https://stackoverflow.com/questions/32910096/is-there-a-way-to-auto-generate-a-str-implementation-in-python
