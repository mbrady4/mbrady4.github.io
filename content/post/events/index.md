+++
title = "Events"

date = 2020-10-14T00:00:00
lastmod = 2020-10-14T00:00:00
draft = false
reading_time = false
authors = ["Michael W. Brady"]
+++
Events are the way we users interact with the page. Any time there is some interaction by way of a mouse, keyboard, etc., the DOM creates and propagates an event object. This event object carries information about the event so that it may be handled at any point up the tree from the point of origin.

Every user interaction with a site is an event: a click, moving the mouse, scrolling the page, pressing a key on the keyboard, these are all events on the page, and the browser can detect all of them. There are tons of different events the browser tracks. When an event happens on a page, it is known as a trigger.

> We can chose specific elements to listen to specific events and fire a callback when that event occurs. This is called an event listener.

## The Event Object

`.addEventListener` — takes two arguments, first the event to listen for and, second, the callback to fire when that event is triggered.

- `element.addEventListener('click', callback);`

The callback (also known as an event handler) will take a single argument; this is known as the `Event Object`. This is a Javascript Object and contains all we need to know about the event and the element it happened on.

- eg: `element.addEventListener('click', (event) => {//Handle event});`
- One of the most important properties of the event object is `.target`, this property will give us all of the info about the DOM node where the event happened. It has many of the same properties as a regular DOM node, `.children`, `.parent`, `.style`, `innerText`, etc. We can use these properties to manipulate the element itself, or it’s relatives.
- We can use this to manipulate the target in any way we want, for example to change the background color we would write the following: `element.addEventListener('click', (event) => { event.target.style.backgroundColor = 'blue'; });`

### Event Propogation

When an event is fired on an element that has parent elements (in this case, the <video> has the <div> as a parent), modern browsers run two different phases — the capturing phase and the bubbling phase. In modern browsers, by default, all event handlers are registered for the bubbling phase.

![Events%20b58cc2196a0f405098119cb22474e7a5/Untitled.png](Events%20b58cc2196a0f405098119cb22474e7a5/Untitled.png)

[Introduction to events](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Building_blocks/Events#Event_bubbling_and_capture)

In our event handler, we are passed the event object. The event object has lots of methods and properties on it including one called `.stopPropagation()` if we call this method in our event handler, it will effectively stop our event from bubbling any further up the chain.

- eg: `const eventHandler = (event) => { event.stopPropagation() };`

### **`.preventDefault`**

Some elements have a native default reaction to certain events. For example, form elements will refresh the page on submit. `.preventDefault` is a method on the event object, and it will stop an HTML element from reacting in its default way. `.preventDefault` will be used less than `.stopPropagation`, but it is important to know about as well.