## Static method / Class method

understand how both of them works

```python
class StaticMethod:
    def __init__(self, function):
        print("inside static __init__")
        self.function = function

    def __get__(self, instance, owner):
        print("inside static __get__")
        return self.function

class ClassMethod:
    def __init__(self, function):
        print("inside class __init__")
        self.function = function

    def __get__(self, instance, owner):
        print("inside class __get__")
        def wrapper(*args, **kwargs):
            print("inside class wrapper __get__")
            return self.function(owner or type(instance), *args, **kwargs)
        return wrapper
```

example usage

```python
class A:
    def thisisstatic(x):
        print(x)

    thisisstatic = StaticMethod(thisisstatic)

    def thisisklass(cls, x):
        print(f"{cls.__name__} {x}")

    thisisklass = ClassMethod(thisisklass)
```

output

```python
print("\n----------A.thisisstatic(1)")
A.thisisstatic(1)
print("\n----------A().thisisstatic(1)")
A().thisisstatic(1)
print("\n----------A.thisisklass(1)")
A.thisisklass(1)
print("\n----------A().thisisklass(1)")
A().thisisklass(1)
```