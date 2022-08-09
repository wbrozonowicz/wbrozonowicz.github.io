---
title: "Simple navigation menu example in REACT"
excerpt: "Simple navigation in REACT with css"
last_modified_at: 2022-08-09T10:27:01-05:00
categories:
  - JavaScript
  - React
  - css
tags: 
  - basics
---

<!-- short introduction -->
## Simple navigation menu in REACT [router and css]

Below very simple navigation menu example [react+css] with some comments:

1. NavigationComponent.js file

{% include code-header.html %}
```js
import React from 'react';
import { NavLink } from 'react-router-dom';
import "../styles/NavigationComponent.css";

const itemList = [
  { name: "home", path: "/", exact: true },
  { name: "page1", path: "/page1", exact: true  },
  { name: "page2", path: "/page2", exact: true },
  { name: "page3", path: "/page3", exact: true },
]

const NavigationComponent = () => {

  const myMenu = itemList.map(item => (
    <li key={item.name}>
      <NavLink to={item.path} exact={item.exact}>{item.name}</NavLink>
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

export default NavigationComponent;
```

2. css file (NavigationComponent.css):

{% include code-header.html %}
```css
nav.main_menu {
    padding: 0 30px 30px 0;
}

.main_menu ul {
    list-style-type: none;
}

nav.main_menu a {
    display: block;
    text-decoration: none;
    color: black;
    text-transform: uppercase;
    font-weight: bold;
    padding: 8px;
    transition: .3s;
}

nav.main_menu a:hover {
    transform: translateX(15px);
}

nav.main_menu a.active {
    border-left: 12 blue solid;
    transform: translateX(0);
}

nav.main_menu li:nth-child(1) a.active {
    border-left-color: orange;
}

nav.main_menu li:nth-child(2) a.active {
    border-left-color: greenyellow;
}

nav.main_menu li:nth-child(3) a.active {
    border-left-color: magenta;
}

nav.main_menu li:nth-child(4) a.active {
    border-left-color: darkolivegreen;
}
```
Some notes:
- "transition: .3s" add animation to element with time = 0.3sec
- "transform: translateX(15px);" in animation element will move 15pixels horizontally
- "transform: translateX(0);" when We use it in a.active it will prevent active element from being animated (turn off animation for it)
- "nav.main_menu a:hover" for hovering with curson on element a
- "nav.main_menu li:nth-child(1) a.active" this will add color border only for 1 element in list (li)


That's all!



