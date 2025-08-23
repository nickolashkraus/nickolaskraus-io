+++
title = 'How to Mock the Built-in open() Function in Python'
date = 2018-03-18T00:00:00-00:00
lastmod = 2025-05-03T00:00:00-00:00
draft = false
description = '''
This article demonstrates how to mock the built-in open() function for a Python
function.
'''
tags = ['programming', 'python', 'testing']
aliases = ['/articles/how-to-mock-the-built-in-function-open/']
+++

In my [previous article][previous article], I demonstrated how to create a unit
test for a Python function that raises an exception. This allows a function to
be tested without its execution being fatal.

Another case where a similar pattern can be applied is when mocking a function.
[`unittest.mock`][`unittest.mock`] is a standard library module for testing in
Python. It allows you to replace parts of your system under test with mock
objects and make assertions about how they have been used.

In this example, we leverage the [`patch()`][`patch()`] function, which handles
patching module- and class-level attributes within the scope of a test. When
using `patch()`, the object you specify will be replaced with a mock (or other
object) during the test and restored when the test ends.

## Problem

Let's say I have a function `open_json_file()`:

```python
def open_json_file(filename):
    """
    Attempt to open and deserialize a JSON file.

    :param filename: name of the JSON file
    :type filename: str

    :return: dict of log
    :rtype: dict
    """
    try:
        with open(filename) as f:
            try:
                return json.load(f)
            except ValueError:
                raise ValueError('{} is not valid JSON.'.format(filename))
    except IOError:
        raise IOError('{} does not exist.'.format(filename))
```

How do I mock `open()` to prevent it from being called with the actual file?
How do I elicit an `IOError` and/or `ValueError` and test the exception
message?

## Solution

To test this behavior, we use [`mock_open()`][`mock_open()`] in conjunction
with [`assertRaises()`][`assertRaises()`]. `mock_open()` is a helper function
to create a mock to replace the use of the built-in function `open()`.
`assertRaises()` allows an exception to be encapsulated, which means that the
test can raise and inspect exceptions without halting execution, as would
normally happen with unhandled exceptions. By nesting both `patch()` and
`assertRaises()` we can simulate the data returned by `open()` and cause an
exception to be raised.

The first step is to create the `MagicMock` object:

```python
read_data = json.dumps({'a': 1, 'b': 2, 'c': 3})
mock_open = mock.mock_open(read_data=read_data)
```

**NOTE**: `read_data` is the string that will be returned by the file handle's
`read()` method. This is an empty string by default.

Next, using `patch()` as a context manager, `open()` can be patched with the
new object, `mock_open`[^1]:

```python
with mock.patch('__builtin__.open', mock_open):
    ...
```

Within this context, a call to `open()` returns `mock_open`, the `MagicMock`
object:

```python
with mock.patch('__builtin__.open', mock_open):
    with open(filename) as f:
        ...
```

In the case of our example:

```python
with mock.patch('__builtin__.open', mock_open):
    result = open_json_file('filename')
```

With `assertRaises()` as a nested context, we can then test for the raised
exception when the file does not contain valid JSON:

```python
read_data = ''
mock_open = mock.mock_open(read_data=read_data)
with mock.patch('__builtin__.open', mock_open):
    with self.assertRaises(ValueError) as context:
        open_json_file('filename')
    self.assertEqual(
        'filename is not valid JSON.', str(context.exception))
```

Here is the full test, which provides full test coverage of the original
`open_json_file()` function:

```python
def test_open_json_file(self):
    # Test valid JSON.
    read_data = json.dumps({'a': 1, 'b': 2, 'c': 3})
    mock_open = mock.mock_open(read_data=read_data)
    with mock.patch('__builtin__.open', mock_open):
        result = open_json_file('filename')
    self.assertEqual({'a': 1, 'b': 2, 'c': 3}, result)
    # Test invalid JSON.
    read_data = ''
    mock_open = mock.mock_open(read_data=read_data)
    with mock.patch('__builtin__.open', mock_open):
        with self.assertRaises(ValueError) as context:
            open_json_file('filename')
        self.assertEqual(
            'filename is not valid JSON.', str(context.exception))
    # Test file does not exist.
    with self.assertRaises(IOError) as context:
        open_json_file('null')
    self.assertEqual(
        'null does not exist.', str(context.exception))
```

[^1]: In Python 3, patch `builtins.open` instead of `__builtin__.open`.

[previous article]: https://nickolaskraus.io/posts/how-to-test-a-function-that-raises-an-exception-in-python
[`unittest.mock`]: https://docs.python.org/3/library/unittest.mock.html
[`patch()`]: https://docs.python.org/3/library/unittest.mock.html#unittest.mock.patch
[`mock_open()`]: https://docs.python.org/3/library/unittest.mock.html#mock-open
[`assertRaises()`]: https://docs.python.org/dev/library/unittest.html#unittest.TestCase.assertRaises
