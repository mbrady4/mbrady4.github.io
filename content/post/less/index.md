+++
title = "LESS"

date = 2020-10-14T00:00:00
lastmod = 2020-10-14T00:00:00
draft = false
reading_time = false
authors = ["Michael W. Brady"]
+++
Preprocessing is simply a more robust syntax for CSS written in a different language. That language is then compiled into normal CSS. 

> Syntax (LESS) —> Compiler (Javascript) —> CSS

Preprocessing extends the ability of CSS by adding abstractions and making CSS easier to use. 

## Starting LESSC

Verify that LESS is installed correctly with: `lessc -v` 

Once in project's root folder, run: `less-watch-compiler less css index.less` (Assumes there is css and less folder. Less folder should contain index.less)

## Variables

Variables are identified with the `@` symbol:

```less
@variable-example:  #FF0000;

.some-class {
  font-size: 14px;
  color: @variable-example; // #FF0000
}
```

## Mixins

Mixins derive their name from the ability to mix different classes. Creating one class and using its properties within another class open new possibilities in CSS and enables improve DRY (don't repeat yourself). 

```less
.bordered (@width) {
    border: @width solid #ddd;

    &:hover {
        border-color: #999;
    }
}

h1 {
    .bordered(5px);
}
```

## Nesting

Nesting keeps code organized and readable. It allows for simpler and more understandable specificity scoping than traditional CSS. 

A good rule of thumb is to avoid going more than four levels deep with nesting. 

```less
.parent {
  color: black;
  font-size: 12px;

  .child {
    color: red;
    font-size: 16px;
  } 
  a {
    color: @link-color;
  &:hover {
      color: @link-hover;
  } 
} // parent
```

It is also good practice to nest @media directly into a selector to increase readability. This allows you to see rather than memorize if a selector has mobile styling or not. 

Although we are nesting the @media, it actually 'bubbles' up to wrap the selector that it's nested in when compiled to CSS. 

## Operations

You can use basic math symbols to compute values. The leftmost unit type will be returned as the unit. 

[An Introduction to CSS Pre-Processors: SASS, LESS and Stylus | HTML Mag](https://htmlmag.com/article/an-introduction-to-css-preprocessors-sass-less-stylus)

## Parametric Mixins

You can use parameters to pass in interpolated values from arguments. 

```less
.custom-layout(@justify, @align) {
	display: flex;
	justify-content: @justify;
	align-items: @align;
}
// usage
.custom-layout(flex-end, center);
```

You should try to only use parametric mixins when there is a clear use case that will be repeatedly helpful. 

## Functions

Functions are pre-built pieces of code from the LESS library. Functions return a value.

One common use case is for changing colors on hover with `darken()` or `lighten()`: 

```less
.btn-class {
  padding: 10px 40px;
  border-radius: 10px;
  background-color: teal;
  color: white;
  cursor: pointer;

  &:hover {
	// The compiled color returned is #006667 
    background-color: darken(teal, 5%)
  }
}
```

## Escaping

Escaping is a niche tool that has a lot of use with media queries. The concept of escaping is that you can use string values stored in variables to help make your code DRY. 

```less
// Escaped values
@tablet: ~"(min-width: 500px)";
@desktop: ~"(min-width: 800px)";

h1 {
  font-size: 2rem;
  padding: 10px;

  @media @tablet {
    padding: 20px;
  }

  @media @desktop {
    font-size: 1.8rem;
    padding: 0 10px;
  }
}
```

Note you cannot escape the "@media" part as it will not be interpreted correctly. 

## Imports

It is not uncommon to see a CSS file with thousands of lines of code. Being able to edit a file like that can breed unexpected results and unintended side effects. We can use imports to control the chaos by building smaller files that flow into one file that can be compiled into CSS.

```less
// The cascade matters!

// Vars and Mixins
@import "variables";
@import "mixins";

// General Styles Here
@import "reset";
@import "general-styles";

// Components Here
@import "header";
@import "t cta";
@import "main-content";
@import "contact";
@import "footer";
```