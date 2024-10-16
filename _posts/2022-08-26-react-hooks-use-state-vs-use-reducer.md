---
title: "React state hooks: useState vs useReducer"
excerpt: "React state hooks: useState vs useReducer"
last_modified_at: 2022-08-22T10:27:01-05:00
categories:
  - JavaScript
  - React
tags: 
  - hooks
---

<!-- short introduction -->
## React state hooks: useState vs useReducer

Both are for state managemant. 
UseState is simple and better for simple state, useReducer is complex and better for complex state (f.e. many properties, nested etc.). 

Below example how to refactor useState to useReducer:
UseState

{% include code-header.html %}
```js
import React, {  useState } from 'react';

const App = () => {
const [counter, setCounter] = useState(0)
const [counterQuantity, setCounterQuantity] = useState(1)

  return (
    <>
      
      <div>
        <p>Current result:{counter}</p>
        <p>Current quantity:{counterQuantity}</p>
        <button onClick={()=>setCounter(counter-counterQuantity)}>Minus</button>
        <button onClick={()=>setCounter(counter+counterQuantity)}>Plus</button>
        <button onClick={()=>setCounter(0)}>Reset</button>
        <br/>
        <button onClick={()=>setCounterQuantity(1)}>1</button>
        <button onClick={()=>setCounterQuantity(5)}>5</button>

      </div>
    </>
  )

}

export default App;

```

Now useReducer. Additionally We will log which button was clicked (btnId).
useReducer has syntax:

- "const [state,dispatch] = useReducer(reducer, initialState)"

Where:
- state - is our state object
- dispatch - function that has action argument, that defines how We will change state
- reducer - function that use action object in switch, and change state. Here is all logic in one place.
- initial state - object for initialising state

Why reducer? Because reducer takes two arguments (state and action) and returns one argument (new state)

{% include code-header.html %}
```js
import React, { useReducer } from 'react';

const reducer = (state, action) => {
  console.log('clicked button ' + action.btnID)
  switch (action.type) {
    case "PLUS":
      return { result: state.result + state.quantity, quantity: state.quantity }
    case "MINUS":
      return { result: state.result - state.quantity, quantity: state.quantity }
    case "RESET":
      return { result: 0, quantity: state.quantity }
    case "SET_VALUE_TO_1":
        return { result: state.result, quantity: 1 }
    case "SET_VALUE_TO_5":
      return { result: state.result, quantity: 5 }
    default:
      return state;
  }
};

const App = () => {
  const [counter, dispatch] = useReducer(reducer, { result: 0, quantity: 1 });

  return (
    <>
      <div>

        <p>Current result:{counter.result}</p>
        <p>Current quantity:{counter.quantity}</p>
        <button onClick={() => dispatch({ type: "MINUS", btnID: 'minusBtn' })}>Minus</button>
        <button onClick={() => dispatch({ type: "PLUS", btnID: 'plusBtn' })}>Plus</button>
        <button onClick={() => dispatch({ type: "RESET", btnID: 'resetBtn'  })}>Reset</button>
        <br />
        <button onClick={() => dispatch({ type: "SET_VALUE_TO_1" , btnID: 'set1Btn' })}>1</button>
        <button onClick={() => dispatch({ type: "SET_VALUE_TO_5" , btnID: 'set5Btn' })}>5</button>
      </div>
    </>
  )

}

export default App;

```

Notes:
- reducer is better for testing and for complex logic, where there are many states (nested objects etc )
- useState is better for simple states f.e. 1, 2 or 3 simple properties

--------------------
{% raw %} 

OK, and now We will combine useRducer with useContext, so We will have "store" in context, and inside it our reducer:
1. File AppContext.js

```js
import React, { createContext, useReducer  } from 'react';

const initialState = { result: 0, quantity: 1 }

const reducer = (state, action) => {
  console.log('clicked button ' + action.btnID)
  switch (action.type) {
    case "PLUS":
      return { result: state.result + state.quantity, quantity: state.quantity }
    case "MINUS":
      return { result: state.result - state.quantity, quantity: state.quantity }
    case "RESET":
      return { result: 0, quantity: state.quantity }
    case "SET_VALUE_TO_1":
        return { result: state.result, quantity: 1 }
    case "SET_VALUE_TO_5":
      return { result: state.result, quantity: 5 }
    default:
      return state;
  }
};

export const AppContext = createContext(
  {
  counter: initialState,
  dispatch: () => null
}
);


const AppContextProvider = ({children}) => {
  const [counter, dispatch] = useReducer(reducer, initialState);
return (
  <AppContext.Provider value = {{counter, dispatch}}>
    {children}
  </AppContext.Provider>
)
}

export default AppContextProvider;
```

2. File App.js will have wrapper, that will provide context to all children (nested components).


```js
import React from 'react';
import AppContextProvider from './AppContext';
import Info from './Info'
import ActionPanel from './ActionPanel'

const App = () => {

  return (
    <>
        <AppContextProvider>
          <Info></Info>
          <ActionPanel></ActionPanel>
      </AppContextProvider>
    </>

  )

}

export default App;
```

We have two children here:
- "Info" will use state of counter (print result and quantity from state) from AppContext
- "ActionPanel" will use dispatch method from AppContext

First "Info.js":

```js
import React, { useContext } from 'react';
import { AppContext } from './AppContext';

const Info = () => {
    const { counter } = useContext(AppContext)

    return (<div>
        <p>Current result:{counter.result}</p>
        <p>Current quantity:{counter.quantity}</p>
    </div>);
}

export default Info;
```


Now "ActionPanel.js":

```js
import React, { useContext } from 'react';
import { AppContext } from './AppContext';


const ActionPanel = () => {
    const { dispatch } = useContext(AppContext)

    return (
        <div>
            <button onClick={
                () => dispatch({ type: "MINUS", btnID: 'minusBtn' })}>Minus</button>
            <button onClick={() => dispatch({ type: "PLUS", btnID: 'plusBtn' })}>Plus</button>
            <button onClick={() => dispatch({ type: "RESET", btnID: 'resetBtn' })}>Reset</button>
            <br />
            <button onClick={() => dispatch({ type: "SET_VALUE_TO_1", btnID: 'set1Btn' })}>1</button>
            <button onClick={() => dispatch({ type: "SET_VALUE_TO_5", btnID: 'set5Btn' })}>5</button>
        </div>
    );
}

export default ActionPanel;
```

That's all!


{% endraw %}




