---
title: "Dynamic css style in React"
excerpt: "Dynamic css style in React"
last_modified_at: 2022-08-14T10:27:01-05:00
categories:
  - JavaScript
  - React
tags: 
  - basics
---

<!-- short introduction -->
## Dynamically changed css style in React

For example: We have two classes in our css file ("App.css"):

```css
.underline_class {
text-decoration: underline;
}

.uppercase_class {
  text-transform: uppercase;
}
```

We want turn them on and off with click. So this is how tp do this - file "App.js"

{% include code-header.html %}
```js
import React, { Component } from 'react';
import '../styles/App.css';

class App extends Component {

  state = {
    underline: false,
    upperCase: false,
  }

  handleClick = ()=> {
    this.setState({
      underline: !this.state.underline,
      upperCase: !this.state.upperCase,
    })
  }

  render() {
    const styles = [];

    if (!this.state.underline) {
      styles.push("underline_class")
    }

    if (!this.state.upperCase) {
      styles.push("uppercase_class")
    }

    return (
      <h1 className={styles.join(" ")} onClick={this.handleClick}>Text</h1>
    );
  }
}

export default App;
```
Notes:
- With every render, for start array styles is empty, so there will be no cssclass applied by default
- Then We check in if statements, what is state and push name of the css class if needed to our array
- in render, we take our array and transorm it to one string with separator " " ("styles.join(" ")")
- this way, if both conditions are true, We will get: className={"underline_class uppercase_class"}

That's all!



