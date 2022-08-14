---
title: "Hook useState in React"
excerpt: "Hook useState in React"
last_modified_at: 2022-08-14T10:27:01-05:00
categories:
  - JavaScript
  - React
tags: 
  - hooks
---

<!-- short introduction -->
## Hook useState in React

Hook is simple way to make functional component (stateless) with state :-).
Class components have longer code and are old-fashioned, so better way is to use hooks in functional components!
No need of class components anymore...! Code is shorter (no render function, no problem with "this", no setState...)

Below example of Class component (old way) with two properties in state (counter and info):

{% include code-header.html %}
```js
import React, { Component } from 'react';

class App extends Component {

  state = {
    counter: 0,
    info: "Default text"
  }

  handleTextClick = () => {
    this.setState({ info: "Custom text" })
  }

  handleCounterClick = () => {
    this.setState({ counter: this.state.counter + 1 })
  }

  render() {
    return (
      <>
        <h1 onClick={handleTextClick}>{this.state.info}</h1>
        <p onClick={handleCounterClick}>{this.state.counter}</p>
      </>
    );
  }
}

export default App;
```


Now the same in function component with hook useState:

{% include code-header.html %}
```js
import React, {useState} from 'react';

const App = () => {
  const [counter, setCounter] = useState(0);
  const [info, setInfo] = useState("Default text")

  handleTextClick = () => {
    setInfo("Custom text")
  }

  handleCounterClick = () => {
    setCounter(counter+1)
  }

    return (
      <>
        <h1 onClick={handleTextClick}>{info}</h1>
        <p onClick={handleCounterClick}>{counter}</p>
      </>
    );
}

export default App;
```
Notes:
- We need to import useState from React ("import React, {useState} from 'react';")
- Every property is in separate hook, so We have to useStates (counter and info)
- first is name of the property, second is setter for it (function to update, naming convention: "set" + "Name of the parameter") f.e. "const [info, setInfo]"
- in useState() brackets there is default value for first render: f.e "useState("Default text")"

That's all!



