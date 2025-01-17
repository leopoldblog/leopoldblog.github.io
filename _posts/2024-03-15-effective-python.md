---
title: Notes of Effective Python (Second Edition)
tags: [Notes, Paper Reading, LLMs]
style: fill
color: info
description: Notes of Effective Python (Second Edition)
---
# Chapter 1: Pythonic Thinking

## Follow the PEP 8 (Python  Enhancement  Proposal  #8) Style Guide

[Python Enhancement Proposal #8](https://www.python.org/dev/peps/pep-0008/)

Using  a  consistent  style  makes  your  code  more  approach-able  and  easier  to  read.

### Whitespace

- Lines should be 79 characters in length or less
- Continuations of long expressions onto additional lines should be indented by **4** extra  spaces from their normal indentation level.
- In a file, functions and classes should be separated by two blank lines.
- In a class, methods should be separated by one blank line.
- In a dictionary, put no whitespace  between each key and colon, and put a single space before the corresponding value if it fits on the same line.
- For type annotations, ensure that there is no separation between the variable name and the colon, and use a space before the type information.
  - name: str = "Alice"
  - age: int = 42
  - numbers: List[int] = [1, 2, 3]

### Naming

- Protected instance attributes: `_leading_underscore`
- Private instance attributes: `__double_leading_underscore`
- Classes (including exceptions) should be in `CapitalizedWord` format
- Module-level constants should be in `ALL_CAPS` format
- Class methods should use `cls` (which refers to the class) as the name of the first parameter

### Imports

- Always put `import` statemetns at the top of a file.
- Orders (each in alphabetical order)
  - standard library modules
  - third-party modules
  - your own modules

## Prefer `enumerate` Over `range`

## Prevent Repetition with Assignment Expressions

```python
count = fresh_fruit.get('apple', 0)
if count >=4:
	make_cider(count)

if (count := fresh_fruit.get('apple', 0)) >=4:
	make_cider(count)
```
