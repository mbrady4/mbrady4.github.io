+++
title = "Sticky Navs"

date = 2020-10-14T00:00:00
lastmod = 2020-10-14T00:00:00
draft = false
reading_time = false
authors = ["Michael W. Brady"]
+++
> Sticky positioning is a hybrid of relative and fixed positioning. The element is treated as relative positioned until it crosses a specified threshold, at which point it is treated as fixed positioned.

With position sticky, the specified elements sticks when the viewport gets to the defined position. 

```less
.nav {
		position: sticky;
		top: 0;
}
```

![Sticky%20Navs%20e2c7873158c54390a27f3f26d0e9ab0d/Untitled.png](Sticky%20Navs%20e2c7873158c54390a27f3f26d0e9ab0d/Untitled.png)

[CSS Position Sticky - How It Really Works!](https://medium.com/@elad/css-position-sticky-how-it-really-works-54cd01dc2d46)