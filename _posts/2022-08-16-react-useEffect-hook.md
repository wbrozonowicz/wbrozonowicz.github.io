---
title: "Hook useEffect in React"
excerpt: "Hook useEffect in React"
last_modified_at: 2022-08-14T10:27:01-05:00
categories:
  - JavaScript
  - React
tags: 
  - hooks
---

<!-- short introduction -->
## Hook useEffect in React

In class components We had lifecycle methods lik componentDidMount etc..
To get similar functionality in function components, use hook "useEffect"
Notes:
- import : "import {useEffect} from 'react';"
- use inside component: 

```js
useEffect(() => {
console.log('handle change here');
return () => {
  console.log('cleanUp function here')
}
}, [myState]);
```

- first argument is a function, that should be fired (f.e. fetch data from API and change state)
- second argument is [], if it is empty - function will be fired only with first render (similar to componentDidMount in Class components). If inside is some variable, function will be fired every time this varaible is modified. There can be many variables here, or just one. If second parameter ("[]") is ommited, function will be fired with evere rerender. This is rather not used in standard applications...
- inside function, could be return of so called "cleanUp function". It is used for unsubscribing effect function (for example clean setInterval). This cleanUp function is fired before new render and start of new effect or on unmount component

Below simple example:

{% include code-header.html %}
```js
import {useState, useEffect} from 'react';

function App() {

const [myState, setMyState] = useState('undefined')

useEffect(() => {
console.log('handle change here');
return () => {
  console.log('cleanUp function here')
}
}, [myState]);

const defineFunc = (input) => {
console.log(input)
let state = myState ==='defined' ?  'undefined':  'defined'
setMyState(state)
}

  return (
    <div className="App">
      <p>state: {myState}</p>
      <button onClick={()=>defineFunc('click')}>Define my state</button>
    </div>
  );
}

export default App;

```

After every click We will have rerender and console will print:
- "click"
- "cleanUp function here"
- "handle change here"

But when We unmount component We will get only:
- "cleanUp function here"


Note: if effect is fired twice with empty array, try to delete in index.js tag: "<React.StrictMode>". 
If it is fired more then one it could also be the effect of :
- This component appears more than once in your page
- Something higher up the tree is unmounting and remounting

That's all!



