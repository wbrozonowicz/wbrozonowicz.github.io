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
- with useCallback child component will not be rerender

OK, example:


{% include code-header.html %}
```js
import React, { useState } from 'react';

import ClickCounter from './ClickCounter';
import ClickCounter2 from './ClickCounter2';

const App = () => {

  const [counter1, setCounter1] = useState(0)
  const [counter2, setCounter2] = useState(0)

  const addClickToClickCounter1 = 
  () => setCounter1(prevValue => prevValue + 1 )

  const addClickToClickCounter2 = 
  () => setCounter2(prevValue => prevValue + 1 )

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

And child components as below:

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

{% include code-header.html %}
```js
import React from 'react';

const ClickCounter2 = ({ callback }) => {
    console.log('i am rendered , on btn2' )
    return (
        <div>
            <button onClick={callback} >Add to2</button>
        </div>);
}

export default React.memo(ClickCounter2);
```

- now ClicCounter 1 and 2 are rerendered every time without need (they are only buttons...), even React.memo not helped this time. Reason: ref of function is different.

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

Now child components (buttons) will be not rerender.

That's all!






