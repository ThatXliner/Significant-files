# ThatXliner's Python code style
## What is this
This is a file that defines and describes the code style I use. It is placed in this repository in case anyone wants to use it for reference.

## The style

### TL;DR
The ThatXliner code style uses [Black][1] to format code and mostly conforms to the [Google style guide][2] (GSG) with the following exceptions:
 - The hard line limit is `90` characters. But it is recommended to keep it to `88` characters.
 - It is preferred to use trusty `flake8` instead of opinionated `pylint`. Though, you can choose otherwise if it your preference.
   (I personally like flake8 because it's `pep8` and `pylint` combined)
 - Use `Black` over `YAPF` because it is more determistic. (If `YAPF` was determistic, I would be using it right now!)

**NOTE: I have not *actually read* the Google style guide (GSG). Therefore, I may not agree on some, or even most, parts of it. I just use the GSG as a starter because this file is still WIP.**

This file is designed to be shorter than the GSG. Because, seriously, who wants to read *that*. Unlike the GSG, this styleguide will be more flexible and shorter, so use your discretion. If you have any questions, please feel free to ask by making an issue.


### Imports

TL;DR: Follow the GSG for this one. Namespaces rule. `import`s from `typing` and `six.moves` are exceptions.

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

**Why?**

Because namespaces rule!

### Filler statements

TD;DR: First off, this section is short. Second, just use `...` if you need to fill something in later. Use `pass` if you don't intend to fill it in.

The ellipsis (`...`) and the `pass` statement are the two main filler statements in python. The golden rule is to use the ellipsis in statements you are planning to fill in later whereas you use `pass` for areas you are not planning to fill.

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


[1]: https://github.com/psf/black "Black's GitHub repo"
[2]: https://google.github.io/styleguide/pyguide.html "Google's python styleguide"
