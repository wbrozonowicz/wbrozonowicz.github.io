---
title: "React useCallback hook"
excerpt: "React useCallback hook"
last_modified_at: 2022-08-28T10:27:01-05:00
categories:
  - JavaScript
  - React
tags: 
  - hooks
---

<!-- short introduction -->
## React useCallback hook

UseCallback hook is similar to useMemo. Typical scenario to use this callback is when:
- We pass in props callback function to child component, and this function is changing value of parent component
- without it, child component will be also rerendered (ref of calback function is changed between rerenders)
- wit useCallback child component will not be rerender

OK, example:


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

  return (
    <>
      <p>clicks 1= {counter.result1}</p>
      <p>clicks 1= {counter.result2}</p>
      {<ClickCounter callback={addClickToClickCounter1} text={'add to 1'}></ClickCounter>}
      {<ClickCounter callback={addClickToClickCounter2} text={'add to 2'}></ClickCounter>}
    </>
  )

}

export default App;
```

And child component as below:

{% include code-header.html %}
```js
import React from 'react';

const ClickCounter = ({ callback, text }) => {
    console.log('i am rendered , on ' + text)
    return (
        <div>
            <button onClick={callback} >{text}</button>
        </div>);
}

export default ClickCounter;
```

- now ClicCounter 1 and 2 are rerendered every time without need (they are only buttons...)

So let's fix it with useCallback hook and React.memo:

{% include code-header.html %}
```js
import React, { useState, useCallback } from 'react';

import ClickCounter from './ClickCounter';
import ClickCounter2 from './ClickCounter2';

const App = () => {

  const [counter1, setCounter1] = useState(0)
  const [counter2, setCounter2] = useState(0)

  const addClickToClickCounter1 = 
  useCallback(() => setCounter1(prevValue => prevValue + 1 ),[])

  const addClickToClickCounter2 = 
  useCallback(() => setCounter2(prevValue => prevValue + 1 ),[])

  return (
    <>
      <p>clicks 1= {counter1}</p>
      {<ClickCounter callback={addClickToClickCounter1} ></ClickCounter>}
      <p>clicks 1= {counter2}</p>
      {<ClickCounter2 callback={addClickToClickCounter2} ></ClickCounter2>}
    </>
  )

}

export default App;
```

Now child1:

{% include code-header.html %}
```js
import React from 'react';

const ClickCounter = ({ callback }) => {
    console.log('i am rendered , on btn1' )
    return (
        <div>
            <button onClick={callback} >Add to1</button>
        </div>);
}

export default React.memo(ClickCounter);
```

And child2:

{% include code-header.html %}
```js
import React from 'react';

const ClickCounter = ({ callback }) => {
    console.log('i am rendered , on btn1' )
    return (
        <div>
            <button onClick={callback} >Add to1</button>
        </div>);
}

export default React.memo(ClickCounter);
```

Now child components (buttons) will be not rerender.

That's all!






