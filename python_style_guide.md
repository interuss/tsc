# InterUSS Python Style Guide

This document describes Python practices or styles that should generally be used
when adding or modifying Python code in InterUSS repositories.  When guidelines
below suggest different actions, the guideline nearest the top of this document
should generally take precedence.

General style guidelines may be found [in a separate document](general_style_guide.md).

## [Black](https://black.readthedocs.io/en/stable/)

Black automatically formats Python code and can be invoked with `make format`.
Its style choices take precedence over all other recommendations here to
maintain the ability to always be confident of not introducing invalid
formatting using `make format`.

## [isort](https://pycqa.github.io/isort/)

isort automatically sort imports in Python code and can be invoked with `make format`.
Its style choices take precedence over all other recommendations here to
maintain the ability to always be confident of not introducing invalid
formatting using `make format`.

It's configured to use a Black compatible format.

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

Docstrings should not have any empty values.  If documentation is not needed
for a particular parameter, return type, etc, that part of the docstring should
be omitted:

Yes (assuming the purpose and usage of `bar` is obvious):
```python
def foo(bar: int, baz: str) -> None:
    """Foos the bar and baz.
    
    Args:
        baz: Name of a thing.
    """
```

No:
```python
def foo(bar: int, baz: str) -> None:
    """Foos the bar and baz.
    
    Args:
        bar:
        baz: Name of a thing.
    """
```

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

## Default Python styles

By default, the Python styles recommended in
[PEP 8](https://peps.python.org/pep-0008/) should be followed.
