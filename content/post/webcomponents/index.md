+++
title = "Web Components"

date = 2020-10-14T00:00:00
lastmod = 2020-10-14T00:00:00
draft = false
reading_time = false
authors = ["Michael W. Brady"]
+++
Components are: Single modular pieces of code. Usually consist of HTML, CSS, and JS. Reusable. DRY. Can stand alone. 

A common practice is to have your preprocessed file named after the component. You could then import your component name into the main file. Here is an example of the import you could use and then what the file could look like:

```
@import custom-btn.less

.custom-btn {
  // custom styles here
}
```

### Example of a simple button component

```jsx
function buttonCreator(buttonText){
    const button = document.createElement('button');

    button.textContent = buttonText;

    button.classList.add('button');

    button.addEventListener('click', (e) => {
        console.log('clicked!');
    });

    return button;
}

let firstButton = buttonCreator('Button 1');

let secondButton = buttonCreator('Button 2');

parent.appendChild(firstButton);
parent.appendChild(secondButton); 
```

Adding components in bulk from arrays:

```jsx
// With .forEach

data.forEach((arrayItem) => {
  let newButton = buttonCreator(arrayItem);

  parent.appendChild(newButton);
});

// With .map

let newComponents = data.map((arrayItem) => {
  let newButton = buttonCreator(arrayItem);

  // Remember, we always need to return something when we use .map
  return newButton;
});

newComponents.forEach(component => {
  parent.appendChild(component);
});
```