# Basics

An object representation of the html elements of a webpage. The DOM is bridge between content and the browser. 

The DOM is not Javscript, HTML, or CSS. 

The DOM is a language neutral API. Tree like structure repenting your content, structure, and style. It is dynamic

> When a web page is loaded into a browser, the browser first looks for the HTML file. The browser uses the HTML file as a blueprint, or instructions on how to build the page (this coupled with the CSS file later). The browser parses these instructions and builds a model for how the page should look and act using Javascript. This model is a Javascript Object containing every element in order on the page. This Object is referred to as the DOM, or Document Object Model.

The DOM is built as a data structure known as a ‘Tree’, because parent elements have nested children elements (or leaves). As with physical trees, we can follow branches of the tree to get to the exact leaf (or leaves) that we want to access. Each branch of our DOM tree can be it’s own tree. It is important to remember this as we move through this lesson.

![Basics%208768f78c546b403b82d482fade6cae11/Untitled.png](Basics%208768f78c546b403b82d482fade6cae11/Untitled.png)

When the DOM is built and the webpage is loaded, developers get access to it in the form of the global Javascript object `document. document` contains the entire hierarchy of the page, each element (or DOM node), and it also contains dozens of built in methods and properties. We can use these methods and properties to manipulate what we see on the screen.

## `getElement` Methods

These elements take a single string as an argument, containing either the id, class, or semantic tag you are looking for:

`document.getElementsByTagName('p');`

`document.getElementById('idName');`

`document.getElementsByClassName('className');`

Each of these methods will return a HTMLCollection containing all the elements that contain the specified name. An HTMLCollection is an array-like object.

## `querySelector` Methods

These methods allow us to select elements based on CSS selectors (e.g., class and ids). 

`document.querySelector('.custom-style');`

`document.querySelectorAll('queryString');`

These methods will search for and return ALL elements matching the query string. This method returns these elements in an array-like object called a NodeList

To convert HTMLCollections and NodeLists to Javascript arrays, we can use:

`Array.from(arrayLikeObject)`

### **`.textContent`**

- Gets and sets the text of an element. Essentially whatever text is between the open and closing tags of an HTML element.
- Can use the assignment operator ( = ) to reset the text of an element
- Setting this property on a node removes all of its children and replaces them with the new single text node.
- `<div>Something Here</div>`
- `element.textContent = 'Something New`;

### **`.setAttribute()` (or .{attr})**

- This method (or property) is used as a way to set or reassign an attribute on the element.
- ‘.setAttribute()’ is a method that takes two arguments, the attribute to set, and the value to set to that attribute.
- eg: `element.setAttribute('src', 'http://www.imagsource.com/image.jpg')`
- Can also use the pattern: `element.'attrName' = ‘value’`.
- eg: `element.src = 'http://www.imagsource.com/image.jpg'`

### **`.style`**

- Every element contains a style object. This property accesses that style object. The style object contains every available style as a key and a value as the value. It is important to note, that these are NOT the CSS styles, these are inline HTML styles.
- These styles are associated with the HTML inline style set on the element
    - eg: `<div style=“color: red;”>DIV STUFF</div>`
- You can access and change a property on the style object by using the assignment operator `=`.
- eg: `element.style.color = ‘blue’;`
- Changing a property on the style object will effectively give this element an inline style.
- Inline styles have the highest specificity, overriding any other selector except `!important`.
- VERY IMPORTANT to note that this does NOT access or change anything in the CSS file.

### **`.className`, `.id`**

- `.className` accesses or assigns a string containing all of the classes on the element.
- `.id` accesses or assigns a string containing the id of the element.

### **`.classList`**

- `classList` will return an array-like object of all the classes on the element. There are many useful methods available on `classList`.
    - `classList` is a `DOMTokenList`.
        - A `DOMTokenList` is an array-like object with a numerical zero-based index, a length property, also the `.contains()` and `.forEach()` methods.
    - Most notably the methods `.add()` `.remove()` and `.toggle()` exist. All three take a single string representing the class.
        - `.add('className')` and `.remove('className')` do as their names indicate.
        - `.toggle('className')` will add the class if it does not exist and remove it if if does.

### **`.appendChild()` and `.prepend()`**

- These methods add child elements to parent elements.
- `.appendChild(child)` will take an element and add it to it’s children. It will add it to the ‘end’ physically so if the children are displayed in order it will be the last.
    - eg: `parentElement.appendChild(childElement)`
- `.prepend(child)` adds a child to the beginning, displaying it first.
    - eq: `parentElement.prepend(childElement)`

### **`.children` and `.parentNode`**

- These properties are used for accessing relatives of the element.
- `.children` returns an `HTMLCollection` of all the children of that element.
- `.parentNode` returns the parent element of that element.

### **`.createElement`**

- `.createElement` creates a brand new element based on a given string.
- New element exists in memory, but not on the DOM yet.
- Can use any DOM property or method to style and manipulate the element.
- eg: `document.createElement('h1')` will create an `h1` element.

### **`.appendChild)` and `.prepend()`**

- Add child elements to parent elements.
- `.appendChild(child)` add an element to it’s children. Adds to the ‘end’, so that if displayed in order, the added child will be last.
    - eg: `parentElement.appendChild(childElement)`
- `.prepend(child)` adds a child to the beginning, displaying it first.
    - eq: `parentElement.prepend(childElement)`