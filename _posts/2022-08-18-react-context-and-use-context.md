---
title: "React context and Hook useContext"
excerpt: "React context and Hook useContext"
last_modified_at: 2022-08-18T10:27:01-05:00
categories:
  - JavaScript
  - React
tags: 
  - hooks
---

<!-- short introduction -->
## React context and Hook useContext

Sharing state before components in React by passing props is problematic (especially when there are many nested components).
To make this easier, We can use React context: this way We will provide context with state (context provider) and use it in different components (context consumers), without passing props!

It's very easy in functional components and a little bit weird in class components, so one more reason to go with functional components in React!

First We have to define class with context. This is just a sheme (structure) of data that will be used and shared, with default data.
Example - file will be "AppContext.js":



```js
import { createContext } from 'react';

export const defaultObject = {
  isUserLoggedIn: false,
  toggleLoggedInState: () => console.log('default provider'),
}

export const AppContext = createContext(defaultObject);
```

Notes:
- We will have to shared properties: the first is boolean value (isUserLoggedIn), the second is function
- by default first value is false, and function is just empty arrow function without implementation

OK, now how to use it in Class component:

First We have to have Context provider - class that Will keep state and share it with other components. We will have this in App.js file:

{% raw %} 
```js
import { PureComponent } from 'react';

import Button from './Button';

import { AppContext, defaultObject } from './AppContext';

class App extends PureComponent {
  state = {
    isUserLoggedIn: defaultObject.isUserLoggedIn,
  }

  render() {
    return (
      <div>
        <AppContext.Provider value={{
          isUserLoggedIn: this.state.isUserLoggedIn,
          toggleLoggedInState: this.handleToggleStateIsLoggedIn
        }}>
          <Button />
        </AppContext.Provider>
      </div>
    );
  }

  handleToggleStateIsLoggedIn = () => this.setState(prevState => ({
    isUserLoggedIn: !prevState.isUserLoggedIn,
  }));

}

export default App;
```


Notes:
- We are wrapping Button component  with our context provider, with some values. This values comes from state (isUserLogged) or from class (function handleToggleStateIsLoggedIn)

Now We want to use these values in Button component (context consumer) without props, below example:

```js
import { PureComponent } from 'react';

import { AppContext } from './AppContext';

class Button extends PureComponent {
  static contextType = AppContext;

  render() {
     const { isUserLoggedIn } = this.context;
     const info = isUserLoggedIn ? 'logged in' : 'logged out'
    return (
      <button onClick={this.context.toggleLoggedInState}>
        { info }
      </button>
    );
  }
}

export default Button;
```

Notes:
- onClick will get function from Context,
- button will show text with value isUserLoggedIn
- this will be all thanks to context! 

Nice but even better is to use this in functional components with hook "useContext".

 Example:

File App.js (context provider):

```js
import { useState } from 'react';

import Button from './Button';

import { AppContext, defaultObject } from './AppContext';

const App = () => {
  const [isUserLoggedIn, setIsUserLoggedIn] = useState(defaultObject.isUserLoggedIn);

  const toggleLoggedInState = () => setIsUserLoggedIn(prevValue => !prevValue);

  return (
    <div>
      <AppContext.Provider value={{ isUserLoggedIn, toggleLoggedInState }}>
      <Button />
      </AppContext.Provider>
    </div>
  );

}

export default App;
```

Nice, now Button component (context consumer):

```js
import { useContext } from 'react';

import { AppContext } from './AppContext';

const Button = () => {
  const { toggleLoggedInState } = useContext(AppContext)
  const { isUserLoggedIn } = useContext(AppContext);

  const info = isUserLoggedIn ? 'logged in' : 'logged out'

  return (
    <button onClick={toggleLoggedInState}>
      {info}
    </button>
  );
}

export default Button;
```

Ok, some notes:
- it's so short and simple that notes are not needed :-)

UPDATE!!!!

But there is also much better way of doing the same!!
We can implement all logic also in AppContext and then use it in other components. How? Below example:

```js
import React, { createContext, useState } from 'react';

export const AppContext = createContext();

const AppContextProvider = ({children}) => {
  const [isUserLogged, setIsUserLogged] = useState(false);
  const toggleLoggedState = () => {
    setIsUserLogged(prevValue => !prevValue)
  }

return (
  <AppContext.Provider value = {{isUserLogged, toggleLoggedState}}>
    {children}
  </AppContext.Provider>
)
}

export default AppContextProvider;
```

What is "children"? These are all components that We will wrap with our AppContextProvider

Now, App.js will look like below:
```js
import Panel from './Panel';
import AppContextProvider from './AppContext';

const App = () => (
    <div>
      <AppContextProvider>
      <Panel/>
      </AppContextProvider>
    </div>
  );

export default App;
```
In Panel (and all other components inside, that are children of AppContextProvider) We can use our state from context:

```js
import React from 'react';
import { useContext } from 'react';
import { AppContext } from './AppContext';

const Panel = () => {
    const { isUserLogged } = useContext(AppContext);
    const { toggleLoggedState } = useContext(AppContext)

    const info = isUserLogged ? 'logged in' : 'logged out'

    return (<div>
        <h1>Is logged?</h1>
        <button onClick={toggleLoggedState}>
            {info}
        </button>
    </div>);
}

export default Panel;

```

That's all!


{% endraw %}



