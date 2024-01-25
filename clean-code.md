- [Meaningful names](#meaningful-names)
- [Functions](#functions)
  - [Niladic functions](#niladic-functions)
  - [Monadic functions](#monadic-functions)
  - [Dyadic arguments](#dyadic-arguments)
- [Comments](#comments)
  - [Good comments](#good-comments)
  - [Bad comments](#bad-comments)

# Meaningful names

1. Don't use single-letter names.
2. Don't use indexed variables such as ´a_1´,...,´a_2´
3. Class names should be nouns, avoid common names and be explicit.
4. Method names should be verbs.
5. Pick one noun per concept. Controller, Manager and Driver might have similar meanings, but only one must be chosen per concept.
6. Pick one verb per purpose. Add as mathematical addition and add as list insertion can be confusing; use it just for one purpose.
7. Ask your self if you are writing code closer to solution domain(technical terms, algorithms, patterns, math) or problem domain(business logic). On each case use the corresponding terms and names.
8. Add meaningful context enclosing in classes, functions or namespaces.
9. Don't add context that is not useful and is repeated everywhere.

# Functions

Functions are the building blocks for programming, writing them properly is worth.

1. Small(try to write functions shorter than 30 lines long).
2. Don't repeat yourself.
3. Do one thing.
   1. Write functions as describing a brief TO paragraph.
   2. Function divided into declarations, initializations and transformations are code smells.
   3. Avoid switch statements, they always do N things.
   4. Have no side effects.
4. Don't mix levels of abstraction.
5. The less function arguments, the better. For OOP try to reduce function arguments using class methods, and third objects.
6. Prefer exceptions to returning error codes.
7. Functions should do something or answer something, but not both.
8. Using try catch blocks should only **try one thing**.
9. Don't use output arguments, since this is confusing.
10. Don't try to follow all the rules at once, refine it instead.

## Niladic functions
The best, but almost always useless.

## Monadic functions
Almost always ask a question about the argument or transform it, but should never do both. The following code is confusing whether it initialize a value or ask if has been already initialized

```python
if set_attribute("username", "robert"):
    do_something()
```

Instead use something like this

```python
if not attribute_exists("username"):
    set_attribute("username", "robert")
```

Create many functions doing one thing, rather than passing a flag argument

## Dyadic arguments
Dyads have no natural ordering, therefore try to include the order in the function name. For example, instead of

```python
assert_equals(expected, actual)
```

create a function called

```python
assert_expected_equals_actual(expected, actual)
```

this avoid checking function definition.

# Comments

Avoid comments always. Code needs to be refactor in order to work, but comments don't therefore it's probably that most of the comments are out of date. Even for disciplined programmers it's better to spend time refactoring code and **explain yourself in code** than refactoring comments.

## Good comments

1. Legal comments referencing external documents about terms and conditions.
2. Explaining regex.
3. Warning of consequences such as dangerous code, or long runs.
4. TODO comments for future refactoring or something imposible TODO at the moment.

## Bad comments

1. Redundant.
2. Position markers with many to split code.
3. Comments that can be replaced by code.
4. Author.
5. Commented-out code.
6. Large comments.