---
title: "Simple webpage structure in REACT"
excerpt: "Simple webpage structure in REACT with css"
last_modified_at: 2022-08-08T10:27:01-05:00
categories:
  - JavaScript
  - React
  - css
tags: 
  - basics
---

<!-- short introduction -->
## Simple webpage structure in REACT

Below very simple webpage structure [react+css] with some comments:

1. app.js file

{% include code-header.html %}
```js
import React, { Component } from 'react';
import '../styles/App.css';
import { BrowserRouter as Router } from 'react-router-dom';
import Header from './Header';
import Navigation from './Navigation';
import Page from './Page';
import Footer from './Footer';

class App extends Component {
  render() {
    return (
      <Router>
        <div className="main_app">
          <header>
            {<HeaderComponent />}
          </header>
          <main>
            <aside>
              {<NavigationComponent />}
            </aside>
            <section className="page_content">
              {<PageComponent />}
            </section>
          </main>
          <footer>{<FooterComponent />}</footer>
        </div>
      </Router>
    );
  }
}

export default App;
```

2. css file:

{% include code-header.html %}
```css
div.main_app {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
  width: 90%;
  margin: 0 auto;

  color: #222;
}

header {
  flex-basis: 30vh;
  position: relative;
  overflow: hidden;
}

main {
  display: flex;
  flex-grow: 1;
  padding: 30px 0;
}

aside {
  min-width: 240px;
  
}
section.page_content {
  flex-grow: 1;
 
}

footer {
  background-color: black;
  color: white;
  padding: 10px 0;
  text-align: center;
  text-transform: uppercase;
}
```
Some notes:
- "display:flex" by default will put all elements in row, to make it in one column, use "flex-direction: column;"
- "flex-grow=1" in display-flex child will icrease size of element to all available space
- "flex-basis: 30vh;" will set height of child element in "display:flex" to 30% of viewport height


That's all!



