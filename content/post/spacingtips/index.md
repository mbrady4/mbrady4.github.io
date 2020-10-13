# Spacing Tips

## What do the block, inline, and inline-block properties do?

- `block` elements expand horizontally to take up a whole line (like a heading). We can apply vertical margin to these.
- `inline` elements expand horizontally just enough to contain their content (like `strong` or `em` elements). We **cannot apply vertical margins to these** and they should usually be placed inside a `block` element.
- `inline-block` elements are like `inline` elements, **but you can apply vertical margins to these** (making them useful for things like buttons).

Images are inline by default. 

## How are height and width calculated?

By default, height/width are calculated by adding together the dimensions of the: 

- Content area
- Padding area
- Border area

If box-sizing is set to the default value (content-box) that means that the formula will be `width (of content) + padding + border`

If we set box-sizing to `border-box` then the formula will be `content + padding + border = width`

Note: Margin is the space between elements and thus never part of the calculation

## What is the difference between padding & margin?

- Margin is the space between elements
- Padding is the space inside an element

What does it mean for margins to collapse?

> When margins collapse, they will combine so that the space between the two elements becomes the larger of the two margins. The smaller margin essentially ending up inside the larger one.

If we have two block elements with margin-bottom: 1em on one element and margin-top: 1.5em on the element directly below it, the total space between the two elements would be 1.5em.

Note: Margins don't collapse when the parent element is set to `display: grid` or `display: flex`