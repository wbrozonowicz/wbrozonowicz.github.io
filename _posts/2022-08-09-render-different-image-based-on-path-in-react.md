---
title: "React router - used to render different image on different path"
excerpt: "Render different image on different path (Rect + router)"
last_modified_at: 2022-08-09T11:27:01-05:00
categories:
  - JavaScript
  - React
  - css
tags: 
  - basics
---

<!-- short introduction -->
## Render different image on different path (Rect + router)

Below very simple example how to render different image based on current path and center image in container:

1. HeaderComponent.js file

{% include code-header.html %}
```js
import React from 'react';
import { Route, Switch } from 'react-router-dom';
import "../styles/HeaderComponent.css";
import img1 from '../images/header1.jpg';
import img2 from '../images/header2.jpg';
import img3 from '../images/header3.jpg';

const HeaderComponent = () => {
    return (
        <>
            <Switch>
                <Route path="/" exact render={() => (
                    <img src={img1} alt="alt1..." />
                )} />
                <Route path="/page1" render={() => (
                    <img src={img2} alt="alt2..." />
                )} />
                <Route path="/page2" render={() => (
                    <img src={img3} alt="alt3..." />
                )} />
                <Route path="/page3" render={() => (
                    <img src={img3} alt="alt3..." />
                )} />
                <Route render={() => (
                    <img src={img1} alt="alt1..." />
                )} />
            </Switch>
        </>
    );
}

export default HeaderComponent;
```

Some comments:
- instead of render component, here We use render property and assign arrow function, that returns img for each route
- Switch element has logic: check each route path and if none of them are true, the last one without specified path will be used (default one for wrong path). If one of route is true, then only this one will be used

Now simple css to center image in container:

2. css file (HeaderComponent.css):

{% include code-header.html %}
```css
header img {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    min-width: 100%;
    min-height: 100%;
}
```
Some notes:
- this css add style to element "header" and img that is inside it, in our project
- top and left will shift image, but used with transform will center it
- min-width and min-height will make img as big as container

That's all!



