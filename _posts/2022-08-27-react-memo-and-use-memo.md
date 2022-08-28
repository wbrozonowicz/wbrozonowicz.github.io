---
title: "React memo vs useMemo hook"
excerpt: "React memo vs useMemo hook"
last_modified_at: 2022-08-22T10:27:01-05:00
categories:
  - JavaScript
  - React
tags: 
  - hooks
---

<!-- short introduction -->
## React memo vs useMemo hook

React.memo and useMemo are for prevent rendering when it is not needed (performance reason).
It is not needed for vary simple components (rendering is fastest than comparing by memo if props changed)

When props are simple properties (not object type) it is better to use React.memo() like below:


{% include code-header.html %}
```js
import React, { useState } from 'react';

import ClickCounter from './ClickCounter';

const App = () => {

  const [counter, setCounter] = useState({ result1: 0, result2: 0 })

  const addClickToClickCounter1 = () =>
    setCounter({ ...counter, result1: counter.result1 + 1 })


  const addClickToClickCounter2 = () =>
    setCounter({ ...counter, result2: counter.result2 + 1 })


  return (
    <>
      <ClickCounter counter={counter.result1} btn={'button1'}></ClickCounter>
      <ClickCounter counter={counter.result2} btn={'button2'}></ClickCounter>

      <button onClick={addClickToClickCounter1}>Add do 1</button>
      <button onClick={addClickToClickCounter2}>Add do 2</button>

    </>

  )

}

export default App;

```

In our app We have two ClickCounter (assign to two buttons)
When We increase clicker for first, second is aslo rendered. To prevent this wrap clicker in React.memo() like below:

{% include code-header.html %}
```js
import React from 'react';

const ClickCounter  = ({counter, btn}) => {
    console.log('i am rendered , clicker assign to ' + btn)
    return ( <div>
        Clicks = {counter} from {btn}
    </div> );
}
 
export default React.memo(ClickCounter);
```

Now click on button1 will not fired rerender of clicker assign to button2.

But when we pass in props not simple value, but object, it will not work properly. In this case, solution is useMemo hook. UseMemo Hook returns a memoized value, so it is generally suitable for expensive functions. Because functional components are also functions in React, so this hook is also very good for prevent rendering such components.

- "App.js":

{% include code-header.html %}
```js
import React, { useState, useMemo } from 'react';

import ClickCounter from './ClickCounter';

const App = () => {

  const [counter, setCounter] = useState({ result1: 0, result2: 0 })

  const addClickToClickCounter1 = () =>
    setCounter({ ...counter, result1: counter.result1 + 1 })

  const addClickToClickCounter2 = () =>
    setCounter({ ...counter, result2: counter.result2 + 1 })

    const clickCounter1 = useMemo(()=>
    <ClickCounter counterState={counter} btn={'button1'}></ClickCounter>, [counter.result1]
    )

    const clickCounter2 = useMemo(()=>
    <ClickCounter counterState={counter} btn={'button2'}></ClickCounter>, [counter.result2]
    )


  return (
    <>
     {clickCounter1}
     {clickCounter2}

      <button onClick={addClickToClickCounter1}>Add do 1</button>
      <button onClick={addClickToClickCounter2}>Add do 2</button>

    </>

  )

}

export default App;
```

- "ClickCounter.js":

{% include code-header.html %}
```js
import React from 'react';

const ClickCounter  = ({counterState, btn}) => {
    console.log('i am rendered , clicker assign to ' + btn)
    return ( 
    <div>
        Clicks = {btn==='button1' ? counterState.result1 : counterState.result2} from {btn}
    </div> );
}
 
export default ClickCounter;
```

Syntax of useMemo is simple:
- first argument in useMemo() is callback that returns component, second is array of dependency
- when we pass object in props, in array of dependency just specify which property of object should couse rerender


Another scenerio:
We have expensive function, that is fired on every render of component. f.e.:

```js
const App = () => {
  const [count, setCount] = useState(0);
  const calculation = expensiveFunction(count);

  const expensiveFunction = (num) => {
// expensive logic
};

 // ... the rest of component

```

To prevent recalculating of expensiveFunction with every render, We can use hook like below:

```js
const App = () => {
  const [count, setCount] = useState(0);
  const calculation = useMemo(() => expensiveFunction(count), [count]);

  const expensiveFunction = (num) => {
// expensive logic
};

 // ... the rest of component

```
Now expensiveFunction will be recalculating only when count will be changed!

That's all!






