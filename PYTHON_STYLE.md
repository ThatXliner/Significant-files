# ThatXliner's Python code style
## What is this
This is a file that defines and describes the code style I use. It is placed in this repository in case anyone wants to use it for reference.

## The style

### TL;DR
The ThatXliner code style uses [Black][1] to format code and mostly conforms to the [Google style guide][2] (GSG) with the following exceptions:
 - The hard line limit is `90` characters. But it is recommended to keep it to `88` characters. 
 - Use `Black` over `YAPF` because it is more determistic and has less options. (If `YAPF` was determistic, I would be using it right now!)
 - Use NumPy-style docstrings

**NOTE: I have not *actually read* the Google style guide (GSG). Therefore, I may not agree on some, or even most, parts of it. I just use the GSG as a starter because this file is still WIP.**

This file is designed to be shorter than the GSG. Because, seriously, who wants to read *that*. Unlike the GSG, this styleguide will be more flexible and shorter, so use your discretion. If you have any questions, please feel free to ask by making an issue.

Also, **do not freak out about migration**. I have made setup.cfg and pyproject.toml files designed for this. Just use them. Relax.


### Imports

TL;DR: Always `import` something. Namespaces rule. `import`s from `typing` and `six.moves` are exceptions.

From the [GSG](https://google.github.io/styleguide/pyguide.html#224-decision):

"2.2.4 Decision
 * Use import x for importing packages and modules.
 * Use from x import y where x is the package prefix and y is the module name with no prefix.
 * Use from x import y as z if two modules named y are to be imported or if y is an inconveniently long name.
 * Use import y as z only when z is a standard abbreviation (e.g., np for numpy).

For example the module sound.effects.echo may be imported as follows:
```python
from sound.effects import echo
...
echo.EchoFilter(input, output, delay=0.7, atten=4)
```
Do not use relative names in imports. Even if the module is in the same package, use the full package name. This helps prevent unintentionally importing a package twice.

Imports from the typing module and the six.moves module are exempt from this rule."

Editing the [`sys.path`](https://docs.python.org/3/library/sys.html#sys.path) should be a last resort (e.g. Importing a module from a folder above).

**Why?**

Because namespaces rule! And it's because ugly/unorganized import statements are bad. Plus, they may contribute to ambiguity, recursive imports, and a whole lot of bugs that we don't want to encounter.

### Filler statements

TL;DR: Just use `...` if you need to fill something in later. Use `pass` if you don't intend to fill it in.

The ellipsis (`...`) and the `pass` statement are the two main filler statements in python. The golden rule is to use the ellipsis in statements you are planning to fill in later whereas you use `pass` for areas you are not planning to fill (e.g. abstract methods or mypy type stubs).

Example:

```python
def some_func() -> ...:
    ...  # Planning to fill later so we use ellipsis

...  # Also used to indicate truncated code
```
And `pass`
```python
import abc


class AbstractClass(metaclass=abc.ABCMeta):
    @abc.abstractmethod
    def draw(self) -> None:
        pass  # This is an abstract method, so we're not planning to fill this in
```

**Why?**

Because we're not gonna have ambiguity over these two wonder statements.

### Comments

TL;DR: Use `#` for comments! (This is for you, @KomodoKode). Use codetags defined in [PEP 350][3] such as `NOTE` or `OPTIMIZE`.

Use the `#` for comments and `"""` for docstrings (`R"""` for `r`aw docstrings).

Codetags, such as `NOTE`, `OPTIMIZE`, `BUG`, `FIXME`, etc, should have the author's username appended to them. From this:

```python
foo = 10
bar = foo + 12
# -snip-
print(bar)
```

to this:

```python
foo = 10
bar = foo + 12
# -snip-
print(bar)  # NOTE(ThatXliner): bar is foo + 12
```

**Why?**

Codetags are awesome for small comments! And we need to see who wrote them because then we can discuss with them.

### Strings

TL;DR: use `f`-strings for string interpolation (if you can), `r`-strings for regexes, `R`-strings for raw strings, and use implicit string concatenation for concatenating long strings.

The general rule for string interpolation is that if you can't use `f`-strings, use `.format`. Like this:

```python
name = "John Doe"
age = 970
print(f"name: {name}, age: {age}")
# OUT: name: John Doe, age: 970
```
or this (if you have those rare cases of having backslashes in string interpolation):

```python
favorite_ascii_character = "\\"
print("My favorite character is '{}'".format(favorite_ascii_character))
# OUT: My favorite character is '\'
```

**Why?**

Personally, I think that `f`-strings are the *cleanest* way to interpolate strings.

Compare this:
```python
name = "John Doe"
age = 970
print(f"name: {name}, age: {age}")
# OUT: name: John Doe, age: 970
```
to this:
```python
name = "John Doe"
age = 970
print("name: {}, age: {}".format(name, age))
# OUT: name: John Doe, age: 970
```
this:
```python
name = "John Doe"
age = 970
print("name: %s, age: %s" % (name, age))
# OUT: name: John Doe, age: 970
```
this:
```python
name = "John Doe"
age = 970
print("name: %(name)s, age: %(age)s")
# OUT: name: John Doe, age: 970
```
or this:
```python
name = "John Doe"
age = 970
print("name: " + name + ", age: " + age)
# OUT: name: John Doe, age: 970
```
### Line Length
TL;DR:  Unlike PEP8, the hard limit for all lines is 90 characters wide (which is also contrasting to black's documentation). The recommended limit is 88 characters.

The line length limit is 90 characters.

When writing docstrings, it is recommended to set your editor's soft wrap setting at 90 and press enter or return at every soft line break. That'll help you wrap strings easier. But it won't really help when you plan to expand them (e.g. docstrings) in the future for it may not be the most optimal setting. So use your own discretion.

If your string is a one liner (meaning it won't have any newlines) you should split it up via implicit string concatenation. For example, change this:
```python
print("Some super-duper long string that will go over the recommended line limit of 80 to 88 characters. This is definitely going to go past the 90 character hard limit.")
```
to this:
```python
print("Some super-duper long string that will go over the recommended line limit of 80 "
"to 88 characters. This is definitely going to go past the 90 character hard limit.")
```
And if you have f-strings, you can just put an f in front of every separate string. It'll concatenate normally, without any errors. The same goes for raw strings. Comments should be wrapped around as well. Turn something like this:
```python
# (90 characters is the width of this comment)============================================
# NOTE: Wrap this comment so that it won't go past the 90 character hard limit. So yeah, this is a long comment
```
into this:
```python
# (90 characters is the width of this comment)============================================
# NOTE: Wrap this comment so that it won't go past the 90 character hard limit. So yeah,
# this is a long comment
```

If your code goes past the 90 character limit, your code is too complicated. Try to simplify it by using built-in functions, separate it into functions, use python's itertools, or use recursion instead.

**Why?**

When writing code, 79 characters may make your code look squished. 90 characters makes most files optimally smaller. It also allows for some nice one-liners such as list comprehensons, etc.

### Type hints
TL;DR: Use them.

Type hints

[1]: https://github.com/psf/black "Black's GitHub repo"
[2]: https://google.github.io/styleguide/pyguide.html "Google's python styleguide"
[3]: https://www.python.org/dev/peps/pep-0350/ "Code tags"
