+++
title = "CSS Wrappers"

date = 2020-10-14T00:00:00
lastmod = 2020-10-14T00:00:00
draft = false
reading_time = false
authors = ["Michael W. Brady"]
+++

To implement a wrapper create a div within which all of the content on the page will sit.

```css
/**
 * 1. Centers the content. Yes, it's a bit opinionated.
 * 2. See the "width vs max-width" section
 * 3. See the "Additional Padding" section
 */

.wrapper {
  margin-right: auto; /* 1 */
  margin-left: auto; /* 1 */

  max-width: 960px; /* 2 */

  padding-right: 10px; /* 3 */
  padding-left: 10px; /* 3 */
}
```
