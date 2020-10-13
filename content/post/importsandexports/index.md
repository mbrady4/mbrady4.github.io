# Imports and Exports

In ES6 there are two kinds of exports:

**Named exports** - for example `export function func() {}` is a named export with the name of `func`. Named modules can be imported using `import { exportName } from 'module';.` In this case, the name of the import should be the same as the name of the export. To import the func in the example, you'll have to use `import { func } from 'module';`. There can be multiple named exports in one module.

`export` exports a single named function that can now be imported into another module using the import keyword

```jsx
export const emphasize = str => {
  return str.toUpperCase();
};
```

**Default export** - is the value that will be imported from the module, if you use the simple import statement `import X from 'module'`. X is the name that will be given locally to the variable assigned to contain the value, and it doesn't have to be named like the origin export. There can be only one default export.

When `export default` is specified (either inline or at the end of the file) the declaration that follows is exported by default and additional modules will need to be exported (and imported) by name.

```jsx
const emphasize = str => {
  return str.toUpperCase();
};

export default emphasize;
```

An advantage of modules is that top-level variables do not pollute the global object. In addition to connecting our project files, bringing in a library or an external component to our project is a matter of downloading it with our CLI and then directly importing it to our file.

A module can contain both named exports and a default export, and they can be imported together using `import defaultExport, { namedExport1, namedExport3, etc... } from 'module';`

[What does "export default" do in JSX?](https://stackoverflow.com/questions/36426521/what-does-export-default-do-in-jsx)

## Imports

Importing a file/module starts with declaring the import keyword followed by the name of the import, then the from keyword followed by the module specifier. The module specifier usually is a file path or an npm module name.

Import examples

- A single named export

`import { a } from './directory/fileName'`

- Multiple named exports

`import { item1, item2, item3 } from './directory/items.js'`

- Exported as default

`import Component from './folderName/Component.js'`

[File path specification](https://www.notion.so/e62fa8a904384ba9b1099f58b1db3e94)