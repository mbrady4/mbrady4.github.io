# Flexbox

Using `display: flex` invokes a whole module of options. A flex container would be an class with the property `display:flex` . Any elements found nested inside our container are now considered flex items. BUT the relationship between flex item and flex container do not go any deeper than just a single level. Any element nested inside of a flex item does not become part of the flexbox module

A flexbox container has two axes: a main axis and a cross axis, which default to this: 

![./Untitled.png](Flexbox%208b76584c7b4c489fb119c2c33e1e5740/Untitled.png)

[How Flexbox works - explained with big, colorful, animated gifs](https://www.freecodecamp.org/news/an-animated-guide-to-flexbox-d280cf6afc35/)

By default, items are arranged along the main axis, from left to right. This can be controlled with `flex-direction: column` The direction is know as the main axis.

- `row` : left to right
- `row-reverse` : right to left
- `column` : top to bottom
- `column-reverse` : bottom to top

### flex-wrap

The default value of flex-wrap is `nowrap` - we can control this behavior to allow for items to wrap instead of being contained. 

- `nowrap` content is constrained to container
- `wrap` wrap content from top to bottom
- `wrap-reverse` bottom to top

### flex-flow

`flex-flow: row wrap;`

Just a way to condense your CSS

### Justify-content

**Justify-content** controls how you align items on the main axis. There are five possibles command for `justify-content` : flex-start, flex-end, center, space-between, and space-around

- `flex-start` places content at the start point of main axis
- `flex-end` places content at the end point of the main axis
- `center` places content at the center of the main axis
- `space-between` content has an even space between each item on the main axis
- `space-around` content has equal space around each item on the main axis
- `space-evenly` content has even space around each item on the main axis

### align-items

The cross-axis is perpendicular to the main-axis. Align controls the cross-axis. 

**align-items** controls how items are aligned. There are five options: flex-start, flex-end, center, stretch, and baseline. 

- `stretch` stretch an item's white space to fill the full flex-container
- `flex-start` items placed at the starting point of the cross axis
- `flex-end` items placed at the end point of the cross axis
- `center` items placed at the center of the cross axis
- `baseline` items align with the bottom base of their text content

### align-content

Controls white space for multiple lines of flex items along the cross-axis.

- `stretch`
- `flex-start`
- `flex-end`
- `center`
- `space-between`
- `space-around`

**align-self** allows you to manually manipulate the alignment of one particular element. 

[A Complete Guide to Flexbox | CSS-Tricks](https://css-tricks.com/snippets/css/a-guide-to-flexbox/)

[Flexbox Cheatsheet Cheatsheet - Joni Bologna](https://jonibologna.com/flexbox-cheatsheet)

![https://jonibologna.com/content/2014/Oct/flexboxsheet.png](https://jonibologna.com/content/2014/Oct/flexboxsheet.png)