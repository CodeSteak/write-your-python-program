# Write Your Python Program

A user-friendly python programming environment for beginners.

The ideas of this environment are based on the great ideas from
[Schreib dein Programm!](https://www.deinprogramm.de/sdp/).

## Quick start

* Step 1. Install the latest version of Python 3.
* Step 2. Install the write-your-python-program extension.
* Step 3. Open or create a Python file. The "RUN" button in the taskbar at the bottom will
  run your file with the teaching language provided by write-your-python-program.

### Troubleshooting

- If the extension cannot find the Python interpreter, you might need to set the path to the
  Python interpreter in the settings of visual studio code.
- By default, pylint is enabled in visual studio code. Pylint will then mark certain variables
  (such as `check`, `Record`, `Enum` and `Mixed`) with a warning because they are not imported explicitly but the extension provides them through some magic. We will fix this in the
  next iteration of the extension, but for now you have the disable pylint in the
  settings of visual studio code.

## Features

Here is a screen shot:

![Screenshot](screenshot.jpg)

When hitting the RUN button, the vscode extension saves the current file, opens
a terminal and executes

~~~
python3 -i /path/to/extension/python/src/runYourProgram.py CURRENT_FILE.py
~~~

This makes the following features available:

### Type Definitions

You can define enums, records and mixed data types and the use them with the type hints of
Python 3. At the moment, such type hints serve only the purpose of documentation. A later iteration
of the "write your python program" extension will use the type hints for dynamic
type checks.

#### Enums

~~~python
Color = Enum('red', 'green', 'blue')
~~~

#### Records

~~~python
Point = Record("Point", "x", float, "y", float)
Circle = Record("Circle", "center", Point, "radius", float)
~~~

You work with a record like this:

~~~python
p = Point(2, 3) # point at x=2, y=3
print(p.x)      # Print 2
~~~

#### Mixed Data Types

~~~python
PrimitiveShape = Mixed(Circle, Square)
~~~

To use recursive types, you need `DefinedLater`:

~~~python
Shape = Mixed(Circle, Square, DefinedLater('Overlay'))
Overlay = Record("Overlay", "top", Shape, "bottom", Shape)
~~~

Case distinction works like this:

~~~python
def workOnShape(s: Shape):
    if Square.isSome(s):
        # s is a Square, do something with it
    elif Circle.isSome(s):
        # s is a Circle, do something with it
    elif Overlay.isSome(s):
        # s is an Overlay, do something with it
    else:
        uncoveredCase()
~~~

If your mixed type is made up of primitive types such as str or int, you can also use
`hasType(ty, value)` for checking if `value` has some type `ty`.

### Tests

Tests are defined via `check`. The first argument of check is the actual result,
then second argument the expected result.

~~~python
check(factorial(4), 24)
~~~
