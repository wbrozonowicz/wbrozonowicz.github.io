---
title: "React router v6 with useNavigation"
excerpt: "React router v6 with useNavigation"
last_modified_at: 2022-08-28T10:27:01-05:00
categories:
  - JavaScript
  - React
tags: 
  - hooks
---

<!-- short introduction -->
## React router v6 with useNavigation

OK, example:

- App.js:
{% include code-header.html %}
```js
import React from 'react';
import {
  BrowserRouter,
  Routes,
  Route,
} from "react-router-dom";

import Navigation from './Navigation';
import StartPage from './StartPage';
import Page1 from './Page1';
import Page2 from './Page2';

const App = () => {

  return (
    <>
      <BrowserRouter>
        <Navigation />

        <Routes>
          <Route path="/" element={<StartPage />} />
          <Route path="/page1" element={<Page1 />} />
          <Route path="/page2" element={<Page2 />} />
        </Routes>
      </BrowserRouter>

    </>
  )

}

export default App;

```


- Navigation.js:

{% include code-header.html %}
```js
import React from 'react';
import { Link } from 'react-router-dom';

const itemList = [
  { name: "home", path: "/"},
  { name: "page1", path: "/page1" },
  { name: "page2", path: "/page2" },
]

const Navigation = () => {

  const myMenu = itemList.map(item => (
    <li key={item.name}>
      <Link to={item.path}>{item.name}</Link>
    </li>
  ))

  return (
    <nav className="main_menu">
      <ul>
        {myMenu}
      </ul>
    </nav>
  );
}

export default Navigation;

```

Now child components (buttons) will be not rerender.

That's all!






