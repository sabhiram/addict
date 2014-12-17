# addict - the Python Dict that's better than heroin.
[![build Status](https://travis-ci.org/sabhiram/addict.svg?branch=master)](https://travis-ci.org/sabhiram/addict) [![Coverage Status](https://img.shields.io/coveralls/sabhiram/addict.svg)](https://coveralls.io/r/sabhiram/addict)

addict is a Python module that gives you a dictionary whose values are both gettable and settable using both attribute and getitem syntax.

**Never again will you have to write code like this**:
```Python
body = {
    'query': {
        'filtered': {
            'query': {
                'match': {'description': 'addictive'}
            },
            'filter': {
                'term': {'created_by': 'Mats'}
            }
        }
    }
}
```
From now on, you may simply write the following three lines:
```Python
body = Dict()
body.query.filtered.query.match.description = 'addictive'
body.query.filtered.filter.term.created_by = 'Mats'
```

###Installing
To install simply type
```sh
pip install addict
```
###Usage
addict inherits from ```dict```, but is way more flexible in terms of accessing and setting its values.
Working with dictionaries have never been more awesome than this! Setting the items of a nested Dict is a *dream*:

```Python
>>> from addict import Dict
>>> my_new_shiny_dict = Dict()
>>> my_new_shiny_dict.a.b.c.d.e = 2
>>> my_new_shiny_dict
{'a': {'b': {'c': {'d': {'e': 2}}}}}
```

###Pruning
It behaves much like a defaultdict, in the way that trying to get a nonexistent key will return a new, empty Dict instance.
So trying to peek at an empty item will result in
```Python
>>> addicted = Dict()
>>> addicted.a = 2
>>> addicted.b.c.d.e
{}
>>> addicted
{'a': 2, 'b': {'c': {'d': {'e': {}}}}}
```
But don't you worry, if you by mistake added some empty Dicts in your Dict, you can recursively delete those by running .prune() on your Dict
```Python
>>> addicted.prune()
{'a': 2}
```

### Stuff to keep in mind
Also, remember that ```int```s are not valid attribute names, so keys of the dict that are not strings must be set/get with the get-/setitem syntax
```Python
>>> addicted = Dict()
>>> addicted.a.b.c.d.e = 2
>>> addicted[2] = [1, 2, 3]
{2: [1, 2, 3], 'a': {'b': {'c': {'d': {'e': 2}}}}}
```
However feel free to mix the two syntaxes:
```Python
>>> addicted.a.b['c'].d.e
2
```

###Attributes like keys, items etc.
addict will not let you override attributes that are native to ```dict```, so the following will not work
```Python
>>> iama_addict = Dict()
>>> iama_addict.keys = 2
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "addict/addict.py", line 53, in __setattr__
    raise AttributeError("'Dict' object attribute '%s' is read-only" % name)
AttributeError: 'Dict' object attribute 'keys' is read-only
```

###Recursive Fallback to dict
The defaultdict-like behaviour of addict, means it is prone to accidental setting of attributes. If you don't feel safe shipping your addict around to other modules, use the to_dict()-method, which returns a regular dict clone of the addict Dict.

```Python
>>> regular_dict = my_addict.to_dict()
>>> regular_dict.a = 2
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'dict' object has no attribute 'a'
```
This is perfect for when you wish to create a nested Dict in a few lines, and then ship it on to a different module. 
```Python
body = Dict()
body.query.filtered.query.match.description = 'addictive'
body.query.filtered.filter.term.created_by = 'Mats'
third_party_module.search(query=body.to_dict())
```
###When is this **especially** useful? 
This module rose from the entirely tiresome creation of elasticsearch queries in Python. Whenever you find yourself writing out dicts over multiple lines, just remember that you don't have to. Use *addict* instead.

###Perks
As it is a ```dict```, it will serialize into JSON perfectly, and with the to_dict()-method you can feel safe shipping your addict anywhere.

###Testing, Development and CI
Pull requests and commits will be automatically run against TravisCI and coveralls. 

The unit tests are implemented in the `test_addict.py` file and use the unittest python framework. Running the tests is rather simple:
```sh
python -m unittest -v test_addict

# - or -
python test_addict.py
```

###Testimonials
@spiritsack - *"Mother of God, this changes everything."*
