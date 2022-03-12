## Make function behaves based on arg type

```py
from functools import singledispatch, update_wrapper

@singledispatch
def fun(arg):
    return 'default'

##########################
@fun.register(int)
def fun_int(arg):
    return 'int'

@fun.register(list)
def fun_list(arg):
    return 'list'

class Model(object):
    pass

@fun.register(Model)
def fun_model(arg):
    return 'model'
##########################

assert fun(3) == 'int'
assert fun([]) == 'list'
assert fun(Model()) == 'model'
assert fun('str') == 'default'  # str type is not registered.
assert fun(0.0) == 'default'  # fload too.
assert fun_int('dummy') == 'int'
assert fun_list('dummy') == 'list'
assert fun(object()) == 'default'  # Using 'instance of object' to test the default behavior.


fun.register(str, lambda arg: 'str')  # Adding for str type.
assert fun('str') == 'str'  # New behavior was registered correctly.
assert fun(0.0) == 'default'  # float type is not still expected.


##########################
class MemberModel(Model):
    def __init__(self, name):
        self.name = name
##########################
assert fun(MemberModel(name='ritsu')) == 'model'  # Yes, it is one of Model.


## Registering a behavior for only the MemberModel.
@fun.register(MemberModel)
def _(arg):
    return 'member: {member.name}'.format(member=arg)
assert fun(MemberModel(name='ritsu')) == 'member: ritsu'


print('--------- Congrats! All tests passed :) ---------')
```

one more example

```py
from functools import singledispatch

class Message(object):
    """ Messages to communicate each robots.
    """
    def __init__(self, body, **metadata):
        self.body = body
        self.metadata = metadata

    def __str__(self):
        return '''body: {self.body}
metadata: {self.metadata}\n'''.format(self=self)

@singledispatch
def generate_message(arg):
    """ Creating Message object for each types.
    """
    raise TypeError('Unexpected type')

@generate_message.register(str)
def str_to_message(arg):
    return Message(arg)

@generate_message.register(dict)
def dict_to_message(arg):
    body = arg.pop('body')
    return Message(body, **arg)

def as_robot(func):
    def wrapped(*args, **kwargs):
        ret = func(*args, **kwargs)
        return generate_message(ret) # singledispatch function
    return wrapped

@as_robot
def antique(message):
    return "Good morning, main Ren."

@as_robot
def neomodel(message):
    return {'body': "I'm here.",
            'enjoyment': 1}

if __name__ == '__main__':
    print('====antique====', end='\n')
    print(antique('dummy message'))
    print('====neomodel====', end='\n')
    print(neomodel('dummy message'))


# ∃! python c.py
# ====antique====
# body: Good morning, main Ren.
# metadata: {}

# ====neomodel====
# body: I'm here.
# metadata: {'enjoyment': 1}
```


## Reference

- https://github.com/hirokiky/demo_singledispatch/blob/main/trial.py
- https://hirokiky-blog.readthedocs.io/2013/12/01/python_advent_calendar_2013.html