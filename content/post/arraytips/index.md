+++
title = "Arrays"

date = 2020-10-14T00:00:00
lastmod = 2020-10-14T00:00:00
draft = false
reading_time = false
authors = ["Michael W. Brady"]
+++
### **BASICS**

```jsx
> a.length
```

```jsx
Result:
```

```jsx
array length
```

```jsx
> a[i]
```

```jsx
Result:
```

```jsx
get index i
```

```jsx
> a[i] = x
```

```jsx
Result:
```

```jsx
store x at index i
```

### **STACK**

```jsx
> a.push(x)
```

```jsx
Result:
```

```jsx
append x to array a
```

```jsx
> a.pop()
```

```jsx
Result:
```

```jsx
remove last element of a
```

### **FOR EACH**

```jsx
> a.forEach(f)
```

```jsx
Result:
```

```jsx
call f on each element
```

### **SLICE**

```jsx
> [10,20,30].slice(1)
```

```jsx
Result:
```

```jsx
[20, 30]
```

```jsx
> [10,20,30].slice(1, 2)
```

```jsx
Result:
```

```jsx
[20]
```

```jsx
> a.slice()
```

```jsx
Result:
```

```jsx
copy array
```

### **MAP**

```jsx
> a.map(f)
```

```jsx
Result:
```

```jsx
[f(a[0]), f(a[1]), ...]
```

### **SLICE WITH NEGATIVE ARGUMENTS**

```jsx
> [10, 20, 30, 40, 50].slice(-2)
```

```jsx
Result:
```

```jsx
[40, 50]
```

```jsx
> [10, 20, 30].slice(-3, -1)
```

```jsx
Result:
```

```jsx
[10, 20]
```

### **JOIN**

```jsx
> [1, 2].join()
```

```jsx
Result:
```

```jsx
'1,2'
```

```jsx
> [1, 2].join('')
```

```jsx
Result:
```

```jsx
'12'
```

```jsx
> [1, 2].join('_')
```

```jsx
Result:
```

```jsx
'1_2'
```

### **CONCAT**

```jsx
> a1.concat(a2)
```

```jsx
Result:
```

```jsx
combine two arrays
```

### **INCLUDES**

```jsx
> [10, 20].includes(10)
```

```jsx
Result:
```

```jsx
true
```

```jsx
> [10, 20].includes(30)
```

```jsx
Result:
```

```jsx
false
```

### **INDEX OF**

```jsx
> [10,20].indexOf(20)
```

```jsx
Result:
```

```jsx
1
```

### **REDUCE**

```jsx
> [1,2].reduce((sum, x) => sum + x)
```

```jsx
Result:
```

```jsx
3
```

### **NEW AND FILL**

```jsx
> new Array(2).length
```

```jsx
Result:
```

```jsx
2
```

```jsx
> new Array(2)[0]
```

```jsx
Result:
```

```jsx
undefined
```

```jsx
> new Array(2).fill('a')
```

```jsx
Result:
```

```jsx
['a', 'a']
```

### **FILTER**

```jsx
> [1, 2].filter(x => x > 1)
```

```jsx
Result:
```

```jsx
[2]
```

```jsx
> [1, 2].filter(x => x < 3)
```

```jsx
Result:
```

```jsx
[1, 2]
```

### **FIND INDEX**

```jsx
> [5, 6].findIndex(x => x === 6)
```

```jsx
Result:
```

```jsx
1
```

### **SORT**

```jsx
> ['a', 'b'].sort()
```

```jsx
Result:
```

```jsx
['a', 'b']
```

```jsx
> [10, 2].sort()
```

```jsx
Result:
```

```jsx
[10, 2]
```

```jsx
> [10, 2].sort((x, y) => x - y)
```

```jsx
Result:
```

```jsx
[2, 10]
```

### **ARRAYS ARE OBJECTS**

```jsx
> typeof ['a', 'b', 'c']
```

```jsx
Result:
```

```jsx
'object'
```

```jsx
> const arr = ['a', 'b', 'c']
arr.five = 5
arr['five']
```

```jsx
Result:
```

```jsx
5
```

### **FIND**

```jsx
> [10, 20].find(x => x > 10)
```

```jsx
Result:
```

```jsx
20
```

```jsx
> [10, 20].find(x => x < 0)
```

```jsx
Result:
```

```jsx
undefined
```

### **SOME AND EVERY**

```jsx
> [10, 20].some(x => x > 10)
```

```jsx
Result:
```

```jsx
true
```

```jsx
> [10, 20].every(x => x > 10)
```

```jsx
Result:
```

```jsx
false
```

### **NEGATIVE ARRAY INDEXES**

```jsx
> const arr = ['a', 'b', 'c']
arr[-1]
```

```jsx
Result:
```

```jsx
undefined
```

```jsx
> const arr = ['a', 'b', 'c']
arr[-1] = 'd'
arr[-1]
```

```jsx
Result:
```

```jsx
'd'
```

### **SHIFT**

```jsx
> a.shift()
```

```jsx
Result:
```

```jsx
remove element 0
```

```jsx
> a.unshift(x)
```

```jsx
Result:
```

```jsx
insert at element 0
```

### **REVERSE**

```jsx
> [1, 2].reverse()
```

```jsx
Result:
```

```jsx
[2, 1]
```

### **REDUCE RIGHT**

```jsx
> ['a','b'].reduceRight((a,x)=>a+x)
```

```jsx
Result:
```

```jsx
'ba'
```

### **EMPTY SLOTS**

```jsx
> new Array(1)[0]
```

```jsx
Result:
```

```jsx
undefined
```

```jsx
> 0 in ['something']
```

```jsx
Result:
```

```jsx
true
```

```jsx
> 0 in new Array(1)
```

```jsx
Result:
```

```jsx
false
```

### **FLAT AND FLATMAP**

```jsx
> [ [1, 2], [3, 4]
].flat()
```

```jsx
Result:
```

```jsx
[1, 2, 3, 4]
```

```jsx
> [ [1, 2], [ 3, [4] ]
].flat(1)
```

```jsx
Result:
```

```jsx
[1, 2, 3, [4]]
```

```jsx
> [ {numbers: [1, 2]}, {numbers: [3, 4]},
].flatMap(obj => obj.numbers)
```

```jsx
Result:
```

```jsx
[1, 2, 3, 4]
```