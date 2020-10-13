# Variables

if you don't have a declaration in front of a variable, it will automatically become a global variable. 

> Rule of Thumb When defining/declaring variables use const until you can’t, then use let.

![Variables%20d6d7079d8ed04fdca6e79028855b6b72/Untitled.png](Variables%20d6d7079d8ed04fdca6e79028855b6b72/Untitled.png)

## Var

- function scoped
- Duplicate identifiers are permitted
- Value is mutable

## Let

One of the key abilities we gain from `let` and `const` is that variables cannot be overridden without throwing an error code. `let` allows us to solve this problem.

`let` variables can be re-assigned but not overridden by another name variable declared with the let keyword.

- Block scoped
- Duplicate identifiers are not permitted
- Value is mutable

## const

The major difference between `let` and `const` is that const is immutable, and its value can never be reassigned.

- Block scoped
- Duplicate identifiers are not permitted
- Value is immutable

![Variables%20d6d7079d8ed04fdca6e79028855b6b72/Untitled%201.png](Variables%20d6d7079d8ed04fdca6e79028855b6b72/Untitled%201.png)

![Variables%20d6d7079d8ed04fdca6e79028855b6b72/Untitled%202.png](Variables%20d6d7079d8ed04fdca6e79028855b6b72/Untitled%202.png)