+++
title = "Useful Python Built-ins"

date = 2019-10-22T00:00:00
lastmod = 2019-10-22T00:00:00
draft = false
reading_time = false

# Authors
authors = ["Michael W. Brady"]
+++
### any()

expression returns true if any element of the iterable is true. False if iterable is empty

    >>> any([1>0,1==0,1<0])
    True
    >>> any([1<0,2<1,3<2])
    False

With lambda functions it is possible to check if any element in an iterable meets a certain condition: 

    any(map(lambda x: str(x) == str(x)[::-1], s))

### all()

returns True if all of the elements of the iterable are true. False if iterable is empty.

    >>> all(['a'<'b','b'<'c'])
    True
    >>> all(['a'<'b','c'<'b'])
    False

### .isalpha(), .isupper(), .islower()

    upper = []
    lower = []
    even = []
    odd = []
    
    def separator(a):
        
        if a.isalpha():
            if a.isupper():
                upper.append(a)
            else:
                lower.append(a)
        else:
            if int(a)%2 == 0:
                even.append(a)
            else:
                odd.append(a)
        return

### ''.join(list)

Collapse all items in a list to a single string

    

### .appned()

append an object to the end of a list

### map()

The map() function applies a function to every member of an iterable and returns the result. It takes two parameters: first, the function that is to be applied and secondly, the iterables.

    >> print (list(map(len, ['Tina', 'Raj', 'Tom'])))  
    [4, 3, 3]

Sort() takes a list and returns a new list with those elements in sorted (ascending order).

    a = [5, 1, 4, 3]
    print sorted(a)  ## [1, 3, 4, 5]
    print a  ## [5, 1, 4, 3]

### Customer Sort with key=

Using the key= attribute we can specify a custom sort for tuples or multi-dimensional lists. One way to think about it is "The key function takes in 1 value and returns 1 value, and the returned "proxy" value is used for the comparisons within the sort" ([Google Education](https://developers.google.com/edu/python/sorting)). 

The key argument accepts functions. For example, this solution to a Hacker Rank challenge passes a function to the key attribute which sorts the 2-D list by the values in a specified index in each list. 

    # Athlete Sort Hacker Rank Challenge Solution
    
    # Function to be passed to Key attribute
    def help(s):
        return int(s[k])
    
    
    athletes, attributes = map(int, input().split(' '))
    
    table = []
    for i in range(athletes):
        row = list(input().split(' '))
        table.append(row)   
    k = int(input())
    
    # Sort function with key specified as a function
    sort = sorted(table, key=help)
    
    for i in sort:
        temp = ''
        for x in i:
            temp += (x + ' ')
        print(temp)

## eval()

The eval expression 'evaluates' a statement or object. 

### Examples

    >>> type(eval("len"))
    <type 'builtin_function_or_method'>
    
    >>> type("len")
    <type 'str'>

### Fractions

    from fractions import Fraction
    
    # Store a tuple of values as a reduced fraction
    f = Fraction(6, 8)
    print(f)
    >>> Fraction(3, 4)
    
    # The Numerator and Denominator can be accessed
    print( f.numerator, f.denominator)

### reduce()

The reduce() function applies a function of two arguments cumulatively on a list of objects in succession from left to right to reduce it to one value.

    >>> reduce(lambda x, y : x + y, [1,2,3])
    6
    
    >>> from fractions import gcd
    # You can provide an initial value as a third argument
    >>> reduce(gcd, [2,4,8], 3)
    1

Take a list of Fractions and return the product: 

    def product(fracs):
        t = Fraction(reduce(lambda x, y: x * y, fracs)) 
        return t.numerator, t.denominator