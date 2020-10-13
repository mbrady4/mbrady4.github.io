# Responsive Design

**Fixed Layouts:** What the early web looked like. CSS widths are hardcoded pixels. Design is desktop only. Horizontal scroll bar appears on smaller screens.

**Fluid Layouts:** The opposite of a fixed layout is one that can expand and contract proportion to most devices. CSS widths are coded with percentages or other fluid units. The trouble with fluid is that you have to adopt one style regardless of device size. Problems include: images getting too small, buttons growing tool large, etc. on extreme screen sizes. 

**Adaptive Layouts:** Adaptive layouts borrow from the speed of a fixed-layout but accommodate different devices at specific breakpoints in design. For example, an adaptive website may have hardcoded pixels used for layout using media queries for different devices.

Adaptive layouts still lacks in accommodating thousands of devices because care was only given to specific designs at certain breakpoints. 

**Responsive Layouts:** Responsive layouts combine features of fixed, adaptive, and fluid websites into one seamless experience. Media queries are used to constrain responsive units so that as the viewport expands or shrinks, you get an experience that looks and functions beautifully across thousands of devices and screen sizes. Design is often divided along desktop, tablet, or phone. Fluid units are used throughout the site. Media queries are used. 

- `min-width` : Rules applied for any browser width greter than the value defined in the query
- `max-width` : Rules applied for any browser width less than the value defined in the query

Responsive uses constraints with % based layouts for that seamless feeling between devices. For example, we can use a css property of max-width to control the maximum width of a container while also providing % flexibility below it. Also, note that any horizontal margin or padding needs to be specified with a responsive unit.

**Desktop First**

![Responsive%20Design%208c90dc4ea3374116bfabb7e513cd5aae/Untitled.png](Responsive%20Design%208c90dc4ea3374116bfabb7e513cd5aae/Untitled.png)

Media Queries

```css
/* Standard Tablet Size */
@media (max-width: 768px){

}
```

## Mobile First

Mobile first implies you will design and code for a mobile device first, and expand layout or features as you gain more screen space toward desktop.

For CSS implementations mobile first usually means your general styles are pointed at the phone and you utilize min-width media queries to layer on more complexity as you grow toward desktop views.

![Responsive%20Design%208c90dc4ea3374116bfabb7e513cd5aae/Untitled%201.png](Responsive%20Design%208c90dc4ea3374116bfabb7e513cd5aae/Untitled%201.png)

**An Opinionated perspective on Media Query Break Points**

You don't want to set break points right at the edge of common devices, rather you want to accommodate a range of sizes. The right breakpoints are: 600px, 900px, 1200px, and 1800px if you plan on giving the giant-monitor people something special.

![Responsive%20Design%208c90dc4ea3374116bfabb7e513cd5aae/Untitled%202.png](Responsive%20Design%208c90dc4ea3374116bfabb7e513cd5aae/Untitled%202.png)

[The 100% correct way to do CSS breakpoints](https://www.freecodecamp.org/news/the-100-correct-way-to-do-css-breakpoints-88d6a5ba1862/)

## Setting the Viewport

- Use the meta viewport tag to control line width and scaling of the browser's viewport
- Include `width=device-width` to match the screen's width in device-independent pixels
- Include `initial-scale=1` to establish a 1:1 relationship between CSS pixels and device-independent pixes
- Ensure your page is accessible by not disabling user scaling

```css
<meta name="viewport" content="width=device-width, initial-scale=1">
```