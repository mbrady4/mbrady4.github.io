# Semantic HTML

W3 Standards: the current *recommended* standard for HTML markup is HTML5. HTML5 comes with more meaningful tags to better describe our code. 

We use the word semantic to to describe tags with meaning. If you don't use proper semantics than you could confuse other developers, accessibility tools, and search engines.

Note: We could make up our own HTML tags, it would be valid HTML, but no one else would understand it! 

<div> tags aren't bad. They are used for creating space, structure, or to style elements. They are also used for presentation markup (when the div does not convey meaning, and other semantic tags describe the content.

```jsx
<section>
	<div>
		<p>Content goes here</p>
	</div>
</section>
```

# CSS Selectors

Selectors are used to gain access to an HTML tag. Selectors carry what are known as specificity weights and these can be used to layer on many styles. 

**Universal selector:**

```jsx
* {
	color:red;
}
```

Will select every element on the page. It is the lowest specificity. 

**Element Selectors:**

```jsx
h1 {
	color:blue;
}
```

More specific than universal, but less specific than all others. 

**Class selectors:**

```jsx
.example {
	color:green;
}
```

Greater than universal and element selectors. Most commonly used way to style elements on webpages.

There can be multiple classes on an HTML element. The lowest class on the pages wins

**Pseudo-classes:**

```jsx
h1:last-child {
	color:organge; 
}
```

useful when a set amount of elements is unknown. More specific than universal, element, and classes.

**ID selectors:**

```jsx
#example {

}
```

Can only have one instance of an ID per page. Used for specific reasons that cannot be accomplished by using classes. Will trump everything except inline and important styles.

**Inline styles:**

Styles allow you to add directly to an element. Trumps everything except important.

**Embedded styles:**

Use HTML `<style>` tags to write CSS directly in HTML file. Has a greater specificity weight than universal, element, classes, pseudo-classes, and ID's. 

[HTML semantics cheat sheet · Web Dev Topics · Learn the Web](https://learn-the-web.algonquindesign.ca/topics/html-semantics-cheat-sheet/)