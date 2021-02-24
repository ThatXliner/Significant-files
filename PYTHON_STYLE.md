# Python code style

## Why
This code style is designed to be super short and concise yet still allowing space for developers to flow with their own, unique, artistic style.

**Line length** will be determined by [Pylint][3]'s default configuration (100).

## The style

### String interpolation
Prefer string interpolation styles in the following order:

1. f-strings
2. `str.format` method
3. C-style string interpolation
4. Binary string concatenation

### Filler statements

Use `...` for code that will be filled in later.

Use `pass` for everything else (e.g. abstract methods, type stubs, etc).

### Comment/docstrings
Use [NumPy-style docstrings](https://numpydoc.readthedocs.io/en/latest/format.html) unless you're documenting utility code or modules.

For utility code, try to keep the docstrings short (i.e. only describing what it does).

For modules, give one line of brief description and (optionally) other lines of long description.

### Name style

While, Pylint will cover this, also try to follow the [naming-cheetsheet](https://github.com/tigthor/naming-cheatsheet) guide.

### Import style


 - Use `import x` for importing packages and modules.
 - Use `from x import y` where `x` is the package prefix and `y` is the module name with no prefix.
 - Use `from x import y as z` if two modules named `y` are to be imported or if `y` is an inconveniently long name.
 - Use `import y as z` only when `z` is a standard abbreviation (e.g., np for `numpy`).
 - Use `from . import submodule, other_submodule` (as opposed to `from package_name import submodule, other_submodule`) for importing submodules relative to the current directory/package.

For example the module `sound.effects.echo` may be imported as follows:

```python
from sound.effects import echo
...
echo.EchoFilter(input, output, delay=0.7, atten=4)
```

Do not use relative names in imports. Even if the module is in the same package, use the full package name. This helps prevent unintentionally importing a package twice.

Imports from the `typing` module, `collections` module, `pathlib` module, and the `six.moves` module are exempt from this rule.

All new code should import each module by its full package name.

Imports should be as follows:

Yes:
```python
# Reference absl.flags in code with the complete name (verbose).
import absl.flags
from doctor.who import jodie

FLAGS = absl.flags.FLAGS
````
```python
# Reference flags in code with just the module name (common).
from absl import flags
from doctor.who import jodie

FLAGS = flags.FLAGS
```
No: (assume this file lives in `doctor/who/` where `jodie.py` also exists)
```python
# Unclear what module the author wanted and what will be imported.  The actual
# import behavior depends on external factors controlling sys.path.
# Which possible jodie module did the author intend to import?
import jodie
```
The directory the main binary is located in should not be assumed to be in `sys.path` despite that happening in some environments. This being the case, code should assume that `import jodie` refers to a third party or top level package named `jodie`, not a local `jodie.py`.

### Type hints

Embed type hints in the code if possible

### Lambdas, higher-order functions, and comprehension constructs

Always prefer a comprehension construct (i.e. list comprehensions, dictionary comprehensions, etc.) **unless**:

 - You are converting every element of a iterable into a specific type (then you should use `map(type_constructor, iterable)`)
 - You are requiring to use `functools.reduce` (that is one of the rare cases lambdas should be used)

Otherwise, most cases where `lambda`s and `map` are present can usually be replaced with a cleaner list comprehension.

For example, you can convert this:

```python
add_1 = list(map(lambda x: x + 1, range(10)))
only_even = list(filter(lambda x: x%2==0, add_1))
```
to
```python
add_1 = [x + 1 for x in range(10)]
only_even = [x for x in add_1 if x%2==0]
```
&#x1F631;

But if you're trying to convert all elements of an iterable into a specific type, do

```python
all_str = map(str, some_iterable)
```
Always attempt to use comprehension constructs when possible.

### Principal of least privilege and simplicity

The principal of least privilege implies simplicity; enforce the principal of least privilege unless complexity is involved.

Our principal of simplicity can be defined as the following:

> Explicit is better than implicit.
> Simple is better than complex.
> Complex is better than complicated.
> Flat is better than nested.
> Sparse is better than dense.
> Readability counts.
> -- Zen of Python, lines 2 to 7

Keep in mind that
> In the face of ambiguity, refuse the temptation to guess.
> There should be one-- and preferably only one --obvious way to do it.

> Although that way may not be obvious at first unless you're Dutch.

<!-- ### The little differences

 - Prefer tuples over lists (unless lists are required or in an asynchronous context).
 - Try not to catch general exceptions
-->

### Conclusion

Recommended formatters and linters

 - [`Black`][1]
 - [`Isort`][2] (with `profile=black`)
 - [`Pylint`][3] (with default configuration)

[1]: https://github.com/psf/black
[2]: https://pycqa.github.io/isort/
[3]: https://www.pylint.org/
