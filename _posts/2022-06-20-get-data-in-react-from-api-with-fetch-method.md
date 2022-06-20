---
title: "Get data from API in React - fetch method & setTimeout"
excerpt: "fetch() method in React & setTimeout() example"
last_modified_at: 2022-06-20T10:27:01-05:00
categories:
  - JavaScript
  - React
tags: 
  - basics
  - AJAX
---

<!-- short introduction -->
## Usage of Fetch() and setTimeout() in React 

Fetch method is very simple method to get data from API (alternative is Axios). Below simple example. 

We have one simple component "Result.js" as below:

{% include code-header.html %}
```js
import React from 'react';
import "./Result.css";

const Result = props => (
  <li>Result is: <strong>{props.english}</strong> Pl: <strong>{props.polish}</strong></li>
)

export default Result;
```

This component will render our data (in form of list element). 
We will use this in App.js.

We will also use (just for example) setTimeout() function to delay execution of getData() function by 5 seconds.

To get data in React the proper place is componentDidMount (after render).
The property "isLoaded" is boolean flag of state of data (loaded or not loaded).
Function "getData" is just simple implementation of fetch():
- url is only one argument of fetch
- response is coming with AJAX so We have to use .then() and parse String that We get to json object
- to parse We use: response => response.json()
- and after this, in next .then() we update data by setState ( get array named "results" from json and set flag isLoaded to true) 
- in render We just use Result component with two props: "english" and "polish"
- depending on flag (isLoaded) We render [] of Result component or just "waiting" string

Below full example:

{% include code-header.html %}
```js
import React, { Component } from 'react';
import './App.css';
import Result from './Result';

class App extends Component {

  state = {
    results: [],
    isLoaded: false,
  }

  componentDidMount() {
    setTimeout(this.fetchData, 5000) // wait 5 seconds -> just for example of timeout function
  }

  getData = () => {
    fetch('api url...')
      .then(response => response.json())
      .then(data => {
        this.setState({
          results: data.results,
          isLoaded: true
        })
      })
  }

  render() {
    console.log("render");
    const results = this.state.results.map(result => (
      <Result key={result.id} english={result.en} polish={result.pl} />
    ))
    return (
      <ul>
        {this.state.isLoaded ? results : "Data is loading, please wait..."}
      </ul>
    );
  }
}

export default App;
```


That's all!



