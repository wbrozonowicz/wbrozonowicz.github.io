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

OK, example of simple routing with useNavigation hook.
- We have App that shows Page1 and Page2 to logged user (also on Navigation) and shows StartPage for all users
- If user define wrong path, app will redirect to StartPage

- App.js:
{% include code-header.html %}
```js
import React, {useState} from 'react';
import {
  BrowserRouter,
  Routes,
  Route,
  Navigate
} from "react-router-dom";

import Navigation from './Navigation';
import StartPage from './StartPage';
import Page1 from './Page1';
import Page2 from './Page2';

const App = () => {

  const [logged] = useState(true)

  return (
    <>
      <BrowserRouter>
        <Navigation />

        <Routes>
          <Route path="/" element={<StartPage />} />
          {logged ? 
          <Route path="/page1" element={<Page1 />} />
        :
         null
        }
          
          <Route path="/page2" element={<Page2 />} />
          {/*  when wrong path -> redirect to "/" */}
          <Route
        path="*"
        element={<Navigate to="/" replace />}
    />
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
import React, { useState } from 'react';
import { Link } from 'react-router-dom';

const itemList = [
    { name: "home", path: "/", forLogged: false },
    { name: "page1", path: "/page1", forLogged: true },
    { name: "page2", path: "/page2", forLogged: true },
]

const Navigation = () => {

    const [logged] = useState(true)


    const myMenu = itemList.map(item => {
        if (!item.forLogged) {
            return (<li key={item.name}>
                <Link to={item.path}>{item.name}</Link>
            </li>
            )
        } else {
            return (!logged ?
                null
                :
                <li key={item.name}>
                    <Link to={item.path}>{item.name}</Link>
                </li>)
        }
    }
    )

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

- StartPage.js:

{% include code-header.html %}
```js
import React from 'react';
import { useNavigate } from 'react-router-dom'

const StartPage = () => {

const navigate = useNavigate()

const handleGoBack = () => navigate(-1);
const handleGoToPage1 = () => navigate('/page1')
const handleGoToPage2 = () => navigate('/page2')

    return (
        <div>
            <p>start</p>
            <button onClick={handleGoBack}>Go back</button> <br />
            <button onClick={handleGoToPage1}>Go to Page1</button> <br />
            <button  onClick={handleGoToPage2}>Go to Page2</button> <br />
        </div>
    );
}

export default StartPage;
```

- Page1.js:

{% include code-header.html %}
```js
import React from 'react';
import { useNavigate } from 'react-router-dom'

const Page1 = () => {

    const navigate = useNavigate()

    const handleGoBack = () => navigate(-1);
    const handleGoToStart = () => navigate('/')
    const handleGoToPage2 = () => navigate('/page2')

    return (
        <div>
            <p> Page 1 </p>
            <button onClick={handleGoBack}>Go back</button> <br />
            <button onClick={handleGoToStart}>Go to Start</button> <br />
            <button onClick={handleGoToPage2}>Go to Page2</button> <br />
        </div>
    );
}

export default Page1;
```

- Page2.js:

{% include code-header.html %}
```js
import React from 'react';

import { useNavigate } from 'react-router-dom'

const Page2 = () => {

    const navigate = useNavigate()

    const handleGoBack = () => navigate(-1);
    const handleGoToPage1 = () => navigate('/page1')
    const handleGoToStart = () => navigate('/')

    return (
        <div>
            <p> Page 2 </p>
            <button onClick={handleGoBack}>Go back</button> <br />
            <button onClick={handleGoToStart}>Go to Start</button> <br />
            <button onClick={handleGoToPage1}>Go to Page1</button> <br />
        </div>
    );
}

export default Page2;
```


That's all!






