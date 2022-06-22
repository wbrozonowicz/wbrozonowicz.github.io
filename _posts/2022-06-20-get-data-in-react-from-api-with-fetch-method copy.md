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
Now let's make fetch with error catching. In this example We will use fetch() when user click button, so the function will have name "handleButtonClick".

- Our URL will be in variable called "API"
- in first then We will check if "ok" property of response = true (We could also check if status = 200). If it is ok, We will return response to next then()
- if it is not true (not return response further) We will throw Error with response.status property
- in next then() We will parse response to json (body from response will be parsed to JavaScript object) and it will be passed to next then()
- in last then() We will assign response to variable user. In this case response is an array with onlu one user (this way comes from API)
- We can now setState, but We want to add this new user to list of already existing users. So We will concat array from state (users) with new array with 1 element (user)
- Because We use previus state and change it, We will use arrow function in setState. This is preffered way of changing existing state (when We use it but with modification)
```js
prevState => ({
          users: prevState.users.concat(user)
        })
```
- The last step is to catch error and print it to console. In this block We can also render some message to user (error description etc)

{% include code-header.html %}
```js
const API = 'api url...';

handleButtonClick = () => {
    fetch(API)
      .then(response => {
        if (response.ok) {
          return response;
        }
        throw Error(response.status)
      })
      .then(response => response.json())
      .then(data => {
        const user = data.results;
        this.setState(prevState => ({
          users: prevState.users.concat(user)
        }))
      })
      .catch(error => console.log(error + " error has happened.."))

  }
  ```

  

That's all!



