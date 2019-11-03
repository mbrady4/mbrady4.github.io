+++
title = "Lambda Functions"

date = 2019-10-22T00:00:00
lastmod = 2019-10-22T00:00:00
draft = false
reading_time = false

# Authors
authors = ["Michael W. Brady"]
+++
Lambda functions are a single expression anonymous function often used as an inline function. In simple words,it is a function that has only one line in its body. It proves very handy in functional and GUI programming.

    >> sum = lambda a, b, c: a + b + c
    >> sum(1, 2, 3)
    6

Lambda functions cannot use the return statement and can only have a single expression. Unlike def, which creates a function and assigns it a name, lambda creates a function and returns the function itself. Lambda can be used inside lists and dictionaries.

### Paired with filters

A filter takes a function returning True or False and applies it to a sequence, returning a list of only those members of the sequence where the function returned True. A Lambda function can be used with filters.

The below returns only elements in the list greater than ten and less than 80:

    >> l = list(filter(lambda x: x > 10 and x < 80, l))

### Paired with Sorted():

    >>> tuples = [(1, 'd'), (2, 'b'), (4, 'a'), (3, 'c')]
    >>> sorted(tuples, key=lambda x: x[1])
    [(4, 'a'), (2, 'b'), (3, 'c'), (1, 'd')]
    
    Bader, Dan. Python Tricks: A Buffet of Awesome Python Features (pp. 71-72). Dan Bader (dbader.org). Kindle Edition.