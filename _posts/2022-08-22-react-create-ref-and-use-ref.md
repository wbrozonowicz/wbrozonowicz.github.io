---
title: "React: createRef and useRef"
excerpt: "React: createRef and useReft"
last_modified_at: 2022-08-22T10:27:01-05:00
categories:
  - JavaScript
  - React
tags: 
  - hooks
---

<!-- short introduction -->
## React createRef and useRef

When We want access some DOM element in React, or check if element exists, We can use one of this two functions. Key differences betwen them:

- useRef is not changed with every rerender, createRef is changed
- on first render useRef.current property is undefined, and with next rerender it is the same ref to DOM element (current property is not reset to undefined with rerender)
- on first and every next render createRef.current is null, after rerender current property is once again assign to DOM element (current property is reset to null with rerender)

When to use it: 
1. access DOM element f.e. set focus after render on input
2. check if DOM element exists

So, simple example of first case: set focus on input after component mount, and log refs created with both functions:

{% include code-header.html %}
```js
import React, { createRef, useEffect, useRef, useState } from 'react';

const App = () => {
  const [counter, setCounter] = useState(0)
  const textInputRef = useRef();
  const textInputRef2 = createRef();


  // this will set focus on first input after first render
  useEffect( () => {
    textInputRef.current.focus()
  }, [])

  // this will set focus on button click
  const setFocusOnMe = () => {
    textInputRef2.current.focus()
  }

  // this is only for rerender
  const updateCounter = () => {
    setCounter(counter => counter + 1)
  }

  // useRef
  console.log('render ' + counter + ' ref1: ')
  // useRef: this will be undefined on the first render,after render and on every next render will be <input.../>  
  console.log(textInputRef.current)

  console.log('-----')

  // createRef
  console.log('render ' + counter + ' ref2: ')
    // useRef: this will be null on every render, after every render will be <input.../>  
  console.log(textInputRef2.current)

return (
  <div>
 {/* this will focus on start */}
  <input ref={textInputRef} type='text'/> 
  <br/>

  {/* this will focus after button click */}
  <input ref={textInputRef2} type='text'/> 
  <button onClick={setFocusOnMe}>set focus</button>
  <br/>
  <button onClick={updateCounter}>rerender</button>
  </div>
)

}

export default App;

```

That's all!






