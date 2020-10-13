# Animation

The `antimation` property in CSS can be used to animate many other CSS properties such as `color`, `background-color`, `height`, or `width` . Each animation needs to be defined with the @keyframes at-rule which is then called with the animation property: 

```css
.element {
  animation: pulse 5s infinite;
}

@keyframes pulse {
  0% {
    background-color: #001F3F;
  }
  100% {
    background-color: #FF4136;
  }
}
```

Each @keyframes at-rule defines what should happen at specific moments during the animation. For example, 0% is the beginning of the animation and 100% is the end.

## Animation Sub-Properties

- `animation-name`: declares the name of the `@keyframes` at-rule to manipulate.
- `animation-duration`: the length of time it takes for an animation to complete one cycle.
- `animation-timing-function`: establishes preset acceleration curves such as `ease` or `linear`.
- `animation-delay`: the time between the element being loaded and the start of the animation sequence.
- `animation-direction`: sets the direction of the animation after the cycle. Its default resets on each cycle.
- `animation-iteration-count`: the number of times the animation should be performed.
- `animation-fill-mode`: sets which values are applied before/after the animation.For example, you can set the last state of the animation to remain on screen, or you can set it to switch back to before when the animation began.
- `animation-play-state`: pause/play the animation.

![Animation%20ef31f63d3a104a69b961ca0f29ffd26d/Untitled.png](Animation%20ef31f63d3a104a69b961ca0f29ffd26d/Untitled.png)

[animation | CSS-Tricks](https://css-tricks.com/almanac/properties/a/animation/)

## Creating a slide in Menu

In webkit-based browsers(Safari and Chrome), -webkit-transform is ignored on inline elements.. Set display: inline-block; to make it work.

[Creating a Smooth Sliding Menu](https://www.kirupa.com/html5/creating_a_smooth_sliding_menu.htm)

```jsx
.menu {
  display:inline-block;
  position: fixed;
  top: 75px;
  left: 0;
  width: 100vw;
  height: 100vh; /* This give the height 100% of the screen or, '100% of View Height' */
  background-color: #81C784;
  border: 1px solid #004D40;
  border-left: none;
  border-bottom: none;
  z-index: 2; /* This set the layer level of the element, it has precendence over initial level of 1*/
  transition-property: transform; 
  transition-duration: 0.3s; 
  transition-timing-function: cubic-bezier(0, .52, 0, 1);
  transform: translate3d(-100vw, 0, 0);
}

// Toggle menu--open class on and off with Javascript
.menu--open {
  transform: translate3d(0vw, 0, 0);
}
```