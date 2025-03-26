# Notes on the conversion to Python 3

## Import Changes

The most significant changes were related to module imports that have been reorganized in Python 3:

```python
# Python 2
import htmlentitydefs
import urlparse
import HTMLParser
```

Changed to:

```python
# Python 3
import html.entities as htmlentitydefs
import urllib.parse as urlparse
import html.parser as HTMLParser
import urllib.request as urllib
```

## String and Unicode Handling

Python 3 handles strings differently from Python 2, with all strings being Unicode by default:

1. Removed the distinction between `unicode` and `str` types
2. Replaced `unichr()` with `chr()` for character handling
3. Simplified string handling code that previously had to check for Python 3

For example:
```python
# Python 2
try:
    return unichr(c)
except NameError: #Python3
    return chr(c)
```

Changed to:
```python
# Python 3
return chr(c)
```

## Division Operator

Changed integer division from `/` to `//` for integer floor division:

```python
# Python 2
nest_count = int(style['margin-left'][:-2]) / self.google_list_indent
```

Changed to:
```python
# Python 3
nest_count = int(style['margin-left'][:-2]) // self.google_list_indent
```

## Iteration and Range

Replaced `xrange()` with `range()`:

```python
# Python 2
for i in xrange(len(self.list)):
```

Changed to:
```python
# Python 3
for i in range(len(self.list)):
```

## Command Line Interface Changes

1. Removed the `version` parameter from `optparse.OptionParser` and added a separate `--version` option
2. Updated I/O handling for binary data using buffer:
   ```python
   # Python 2
   data = sys.stdin.read()
   ```
   
   Changed to:
   ```python
   # Python 3
   data = sys.stdin.buffer.read()
   ```

## URL Handling

Updated URL opening to use the new library:

```python
# Python 2
j = urllib.urlopen(baseurl)
```

Changed to:
```python
# Python 3
j = urllib.request.urlopen(baseurl)
```

## Dictionary and Existence Checks

Python 2 code often used `dict.has_key()` which has been removed in Python 3. The `has_key()` function was maintained for backward compatibility:

```python
# Helper function to replace dict.has_key()
def has_key(x, y):
    return y in x
```

## Helper Methods

Added an explicit `out()` method to help with proper output text handling:

```python
def out(self, s):
    """Appends the string to the output list"""
    self.outtextlist.append(s)
```

## Type Checking and Encoding

Updated code that deals with text encoding:

```python
def wrapwrite(text):
    if isinstance(text, str):
        text = text.encode('utf-8')
    try:
        sys.stdout.buffer.write(text)
    except AttributeError:
        sys.stdout.write(text)
```

These changes ensure that the html2text tool functions correctly in Python 3 environments while maintaining all the original functionality of converting HTML documents to Markdown format.


## Fixup

1. Fixed the syntax warnings about improper use of identity operators:
   - Changed `if c is not ' ' and c is not '  '` to `if c != ' ' and c != '\t'`
   - Changed `return c is ' '` to proper whitespace checking

2. Fixed the `onlywhite` function which was both syntactically incorrect and logically flawed:
   - The original was checking against a string of two spaces (`'  '`) which doesn't make sense when iterating over single characters
   - Replaced it with a proper implementation that checks for whitespace characters

3. Removed a duplicated section of code (there was a second copy of the `handle_tag` method in the original file)

4. Fixed the `optwrap` method return statement (it was incorrectly returning `result[-1][2]` instead of just `result`)

5. Changed `xrange` to `range` for Python 3 compatibility

6. Fixed string handling for Python 3 (particularly in handling encoding/decoding)
