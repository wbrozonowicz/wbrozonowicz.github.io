---
title: "Working on previous state in REACT"
excerpt: "Working on previous state in REACT"
last_modified_at: 2022-07-12T10:27:01-05:00
categories:
  - JavaScript
  - React
tags: 
  - basics
---

<!-- short introduction -->
## Working on previous state in REACT with prevState

When We want to change existing state (modify it somehow) We can use method with prevState. Let's see:
- We have in our state array called "elements"
- We want to modify it by adding new element
- We use in setState function, that takes one argument (prevState) and returns object (new array of elements)
- We create new array, inside We use rest operator (...) on array from prevState to make copy of array, and add new element
It's very short and elegant solution. It is important to have () after "prevState => " because this function returns something (object).

{% include code-header.html %}
```js
 this.setState(prevState => ({
      elements: [...prevState.elements, newElement]
    }))
```

That's all!



