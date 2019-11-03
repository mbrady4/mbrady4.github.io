+++
title = "Regex"

date = 2019-10-22T00:00:00
lastmod = 2019-10-22T00:00:00
draft = false
reading_time = false

# Authors
authors = ["Michael W. Brady"]
+++
### re.match()

If zero or more characters **at the beginning of string match** the regular expression pattern, return a corresponding MatchObject instance. Return None if the string does not match the pattern; note that this is different from a zero-length match.

### re.search()

Scan through string **looking for a location** where the regular expression pattern produces a match, and return a corresponding MatchObject instance. Return None if no position in the string matches the pattern; note that this is different from finding a zero-length match at some point in the string

Example: Can a string be converted to a float?

    import re
    
    def is_float(test):
        result = re.match(r'(\+|\-)?\d*\.\d+$', test)
        if result == None:
            return False
        else:
            return True

Example: Check to see if an email is valid

    import re
    
    def valid_email(email):
        valid = re.match(r'[a-z0-9A-Z_\-]+@[a-z0-9]+\.\w{1,2}$', email) != None
        return valid

### re.split()

The re.split() expression splits the string by occurrence of a pattern

    >>> import re
    >>> re.split(r"-","+91-011-2711-1111")
    ['+91', '011', '2711', '1111']

### re.findall()

The expression re.findall() returns all the non-overlapping matches of patterns in a string as a list of strings.

    >>> import re
    >>> re.findall(r'\w','http://www.hackerrank.com/')
    ['h', 't', 't', 'p', 'w', 'w', 'w', 'h', 'a', 'c', 'k', 'e', 'r', 'r', 'a', 'n', 'k', 'c', 'o', 'm']

### Lookarounds

A lookahead or a lookbehind does not "consume" any characters on the string. This means that after the lookahead or lookbehind's closing parenthesis, the regex engine is left standing on the very same spot in the string from which it started looking: it hasn't moved.

**(?<= … ) and (?= … ), (?<! … ) and (?! … )**

**Lookbehind Before the match:**

`(?<=USD)\d{3}`

**Explanation:**

The lookbehind (?<=USD) asserts that at the current position in the string, what precedes is the characters 'USD'. If the assertion succeeds, the engine matches three digits with \d{3}.

## Example

    # Find non-overlapping occurences of 2+ vowels in a row
    import re
    
    chars = 'abaabaabaabaae' #str(input())
    consonants = '[bcdfghjklmnpqrstvwxyz]'
    vowels = '[aeiou]'
    regex = '(?<=' + consonants +')(' + vowels + '{2,})' + consonants
    initial = re.findall(regex, chars, re.IGNORECASE)
    if len(initial) == 0:
        print(-1)
    else: 
        for i in initial: 
            result = re.findall(r'[aeiou]+', i, re.IGNORECASE)
            print(result[0])

[RegExr: Learn, Build, & Test RegEx](https://regexr.com)

[Regex Cheat Sheet](https://www.rexegg.com/regex-quickstart.html)