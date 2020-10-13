# Units

## px

The `px` unit is referred to as an absolute length unit. The `px` unit does not adjust for its surroundings (does grow with zoom features on browsers). Pixels are not responsive nor accessible. 

Most browsers set to default to 16px font-size. 

## em

The `em` unit is referred to as relative length unit. This means that `em` will adjust to its surroundings. 

One of the issues with `em` is its inherited calculation. Changing just one parent element can lead to a cascade of skewed sizes. 

- Responsive to its surroundings
- Can be too flexible
- Converting `px` to `em` when creating code from a design file usually requires a calculator or memorization that can feel cumbersome

## rem

The `rem` or `root em` unit is also referred to as a `relative length unit`. The root part means that htis unit looks to the root element in a page. Usually the root element is going to be the html tag in a web page or application. 

This is a great thing because now we can adjust our relative font-size off of one place instead of many like the em does.

> **Pro Tip:** Using a hard coded pixel on the `html` tag is bad practice as it overwrites the user’s default settings on their browser.

Instead, use a % based unit to be more accessible. A great trick is to use `62.5%` as the base value because then you can convert `rem` units by the power of `10` instead of `16`. `62.5 * .16 = 10`. That means `1rem` would equate to `10px` throughout the document!

Examples:

```
18px === 1.8rem 13px === 1.3rem 27px === 2.7rem
```

## %

The % unit is also referred to as a relative length unit. The % unit us usually used for building responsive layouts more than for font sizing. % based layouts start at the viewport width and then cascade down through nested units. 

- You have to be careful with `%` based layout as widths can get too small very fast
- If you don’t add everything up to 100% when building layout, `%` can cause a lot of frustration searching the box model to see where the math went wrong

## vw / vh

These units completely rely on the viewport of the browser window to render. While these units can be used most places any other unit can be used, we caution against using them except in a specific use-case: full screen layouts.