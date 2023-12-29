# InterUSS General Style Guide

This document describes general practices or styles that should generally be
used when adding or modifying content in InterUSS repositories.

A guide specifically for Python code may be found in
[a separate document](python_style_guide.md).

## Magic strings

Generally avoid repeating the same string literal in multiple places.  If a
string is used in many places, it should generally be defined as a
[constant](https://peps.python.org/pep-0008/#constants) and then that constant
should be used instead.  This allows all instances to be discovered easily with
an appropriate IDE, and it ensures that no typos are made in the literals.

Docstrings for non-obvious constants are encouraged.

## Magic values

When the reason a numeric or other value has the value it has is not
immediately obvious, define and document a constant and use the
constant rather than the magic value.  When a constant is defined in a standard
package, prefer that pre-existing definition rather than defining a new
constant.

No:

```python
foo = bar * 4.721
```

Yes:
```python
FOOS_PER_BAR = 4.721
"""Number of foos in each bar."""

foo = bar * FOOS_PER_BAR
```

No (when this limit is already defined in a standard package):
```python
FOO_LIMIT = 50
"""Maximum number of foos according to Foo Standard."""
```

Yes:
```python
from uas_standards.foo_standard.v1.constants import FOO_LIMIT
```

## What and why, not how

Function names should generally capture what is being done (often hinting at
why) rather than how it is being done.  The focus should be the outcome rather
than the method by which that outcome is obtained.

| Yes                           | No |
|-------------------------------| --- |
| send_subscriber_notifications | send_foo_post_request_to_urls |
| expect_client_notified        | expect_post_interaction_to_client |
| op_intent_details_queried     | interactions_contains_get_request | 

## Don't repeat yourself

See [Wikipedia article](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself).

In general, the same complex logic should not appear in more than one place as
this introduces the possibility of inconsistency and reduces reusability.
Instead, the complex logic should be encapsulated in a function that describes
the concept the logic is embodying, and then that function should be called
from the multiple places the duplicated logic used to reside.

## Consistent naming style

When referring to a particular entity, the name of that entity should be as consistent as possible.  Many naming conventions exist, but once one has been selected, it should be used as consistently as possible.  Namings for different entities of a similar type within the same document should generally follow the same convention.  The most common naming styles/conventions include:

### Proper name

Documentation:
* ✅ "X will intersect with Conflicting Flight 1."
* ✅ "Conflicting Flight 1 will start at X."
* ❌ "Conflicting flight 1 will start at X."
* ❌ "Conflicting_Flight_1 will start at X."
* ❌ "Conflicting_flight_1 will start at X."
* ❌ "X will intersect with Conflicting flight 1."
* ❌ "X will intersect with conflicting flight 1."
* ❌ "X will intersect with conflicting flight1."
* ❌ "X will intersect with conflicting_flight_1."
* ❌ "X will intersect with Conflicting_Flight_1."
* ❌ "X will intersect with `Conflicting Flight 1`."

Code:
* ✅ `conflicting_flight1` or `conflicting_flight_1` (but select and use only one of these options)

### Description

Documentation:
* ✅ "X will intersect with conflicting flight 1."
* ✅ "Conflicting flight 1 will start at X."
* ❌ "X will intersect with Conflicting Flight 1."
* ❌ "X will intersect with Conflicting flight 1."
* ❌ "X will intersect with conflicting flight1."
* ❌ "X will intersect with conflicting_flight_1."
* ❌ "X will intersect with Conflicting_Flight_1."
* ❌ "X will intersect with `conflicting flight 1`."

Code:
* ✅ `conflicting_flight1` or `conflicting_flight_1` (but select and use only one of these options)

### Code object

(note that `conflicting_flight1` could be used instead of `conflicting_flight_1` in all ✅ options below, but whichever option is selected should be used consistently)

Documentation:
* ✅ "X will intersect with conflicting_flight_1."
* ✅ "X will intersect with `conflicting_flight_1`."
* ✅ "conflicting_flight_1 will start at X."
* ✅ "`conflicting_flight_1` will start at X."
* ❌ "X will intersect with conflicting flight 1."
* ❌ "X will intersect with Conflicting Flight 1."
* ❌ "X will intersect with Conflicting_Flight_1."
* ❌ "Conflicting Flight 1 will start at X."
* ❌ "Conflicting_Flight_1 will start at X."
* ❌ "Conflicting_flight_1 will start at X."

Code:
* ✅ `conflicting_flight_1`
