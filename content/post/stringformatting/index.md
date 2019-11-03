+++
title = "String Formatting"

date = 2019-10-22T00:00:00
lastmod = 2019-10-22T00:00:00
draft = false
reading_time = false

# Authors
authors = ["Michael W. Brady"]
+++
> If your format strings are user-supplied, use Template Strings to avoid security issues. Otherwise, use Literal String Interpolation if you’re on Python 3.6+, and “New Style” String Formatting if you’re not.

## String Literals

Pass the variable directly into the string

    >>> f"Hey {name}, there's a {errno:#x} error!"
    "Hey Bob, there's a 0xbadc0ffee error!"

## Old-Style

Use a %s to specify python should insert the value of a specified variable:

    >>> 'Hello, %s' % name
    'Hello, Bob'

If inserting multiple variables, you can insert the variable inline and create a dictionary as follows: 

    >>> 'Hey %(name)s, there is a 0x%(errno)x error!' % {"name": name, "errno": errno }
    'Hey Bob, there is a 0xbadc0ffee error!'