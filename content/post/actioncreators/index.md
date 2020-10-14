+++
title = "Action Creators"

date = 2020-10-14T00:00:00
lastmod = 2020-10-14T00:00:00
draft = false
reading_time = false
authors = ["Michael W. Brady"]
+++

Since action creators are just regular JavaScript functions, you can define them any way you like.

```sql
let nextTodoId = 0; 
export const addTodo = (text) => {
  return {
    type: 'ADD_TODO',
    id: (nextTodoId++).toString(),
    text,
  };
};
```

If the statement it contains is just a return statement, then you can just use the value returned as the body of the function

```sql
let nextTodoId = 0; 
export const addTodo = (text) => ({
  type: 'ADD_TODO',
  id: (nextTodoId++).toString(),
  text,
});
```