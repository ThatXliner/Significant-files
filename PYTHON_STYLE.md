# ThatXliner's Python code style
## What is this
This is a file that defines and describes the code style I use. It is placed here in case anyone wants to use it for reference.

## The style

### TL;DR
The ThatXliner code style uses [Black][1] to format code and mostly conforms to the [Google style guide][2] with the following exceptions:
 - The hard line limit is `90` characters. But it is recommended to keep it to `88` characters.
 - It is preferred to use `flake8` instead of `pylint`. Though, you can choose otherwise if it your preference.
   (I personally like flake8 because it's `pep8` and `pylint` combined)
 - Use `Black` over `YAPF` because it is more determistic. (If `YAPF` was determistic, I would be using it right now!)

> "I love implicit string concatenation"

[1]: https://github.com/psf/black "Black's GitHub repo"
[2]: https://google.github.io/styleguide/pyguide.html "Google's python styleguide"
