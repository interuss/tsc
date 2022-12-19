# InterUSS Python Style Guide

This document describes Python practices or styles that should generally be used
when adding or modifying Python code in InterUSS repositories.  When guidelines
below suggest different actions, the guideline nearest the top of this document
should generally take precedence.

## [Black](https://black.readthedocs.io/en/stable/)

Black automatically formats Python code and can be invoked with `make format`.
Its style choices take precedence over all other recommendations here to
maintain the ability to always be confident of not introducing invalid
formatting using `make format`.

## Type annotations

The following should almost always have type annotations according to
[PEP 484](https://peps.python.org/pep-0484/):

* Arguments in function or method declarations
* Return values of functions or methods
    * Exception: when it is obvious the function or method has no return value,
      the correct return type of `None` may be omitted (though annotating
      non-returning functions or methods with `-> None` is still encouraged)
* Class member variables
* Complex or confusing local variables within a function
  ([PEP 526](https://peps.python.org/pep-0526/))

The type `Any` should usually be avoided if a more specific type is known.

## Function documentation

When the purpose or behavior of a function or method, its input parameters, or
its result is not reasonably obvious from its name and
[typing](#type-annotations), it should at least be:

1. given a docstring explaining at least the components (input parameters,
   return value, behavior of function) that are not obvious, or
2. annotated as private or internal with a single leading underscore in its
   name.

Docstrings should be via [PEP 257](https://peps.python.org/pep-0257/).
[Google format](https://google.github.io/styleguide/pyguide.html#383-functions-and-methods)
is encouraged.

Documentation should not consist solely of a reiteration of a component's name.
If that is sufficient documentation, then comment-based documentation  is not
necessary.

## Data structures

Avoid using `dict`s with fixed key names for structured data.  For data
structures that will not need to be serialized or deserialized, annotate a class
as a [`dataclass`](https://docs.python.org/3/library/dataclasses.html).  For
data structures that may need to be serialized or deserialized, create a class
that inherits from [`ImplicitDict`](https://github.com/interuss/implicitdict).
Avoid named tuples unless performance is important.

No:
```python
my_plain_dict = {"field1": "foo", "field2": "bar"}
x = my_plain_dict["field1"]
y = my_plain_dict["field2"]
```

Yes:
```python
from dataclasses import dataclass

@dataclass
class MyObject(object):
    field1: str
    field2: str

my_dataclass = MyObject(field1="foo", field2="bar")
x = my_dataclass.field1
y = my_dataclass.field2
```

Yes:
```python
from implicitdict import ImplicitDict

class MyObject(ImplicitDict):
    field1: str
    field2: str

my_implicitdict = MyObject(field1="foo", field2="bar")
x = my_implicitdict.field1
y = my_implicitdict.field2
```

## Magic strings

Generally avoid repeating the same string literal in multiple places.  If a
string is used in many places, it should generally be defined as a
[constant](https://peps.python.org/pep-0008/#constants) and then that constant
should be used instead.  This allows all instances to be discovered easily with
an appropriate IDE, and it ensures that no typos are made in the literals.

Docstrings for non-obvious constants are encouraged.

## Default Python styles

By default, the Python styles recommended in
[PEP 8](https://peps.python.org/pep-0008/) should be followed.
