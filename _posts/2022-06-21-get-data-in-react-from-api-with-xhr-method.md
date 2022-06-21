---
title: "Get data from API in React - XMLHttpRequest method"
excerpt: "XMLHttpRequest method in React example"
last_modified_at: 2022-06-21T10:27:01-05:00
categories:
  - JavaScript
  - React
tags: 
  - basics
  - AJAX
---

<!-- short introduction -->
## Usage of XMLHttpRequest in React 

XMLHttpRequest is  old method to get data from API (alternative for fetch() or Axios). Below simple example. 


We will use XMLHttpRequest in App.js to get data from API https://jsonplaceholder.typicode.com/users and render results.



To get data in React the proper place is componentDidMount (after render)!

Function "requestDataByXHR" is just simple implementation of XMLHttpRequest:
- first We need to create object "xhr"
- in next step, we will use function open() of this object with 3 parameters: - http method (GET), url of API, and boolean parameter (true -> async, false -> sync)
- next We will define what will happened when We get response from server. For this purpose We will use method "onload"
- if response has status 200, We Will update our state with parsed data (in state We have to define object named "users")
- We have to also use function send() of xhr object, that will send request. We do not send any data so parameter is null (can be also empty).

Below example of this function:

{% include code-header.html %}
```js
  requestDataByXHR() {
    const xhr = new XMLHttpRequest();
    xhr.open('GET', 'https://jsonplaceholder.typicode.com/users', true);
    xhr.onload = () => {
      if (xhr.status === 200) {
        const users = JSON.parse(xhr.response)
        this.setState({ users })
      }
    }
    xhr.send(null)
  }
```

This function We have to use in componentDidMount (for initial data request) or execute this later by click etc..

There is also the second way possible. Instead of  xhr.onload We can use xhr.addEventListener() function with "load" parameter, to get the same results. Below example

{% include code-header.html %}
```js
  requestDataByXHR() {
    const xhr = new XMLHttpRequest();
    xhr.open('GET', 'https://jsonplaceholder.typicode.com/users', true);
      xhr.addEventListener('load', () => {
      if (xhr.status === 200) {
        const users = JSON.parse(xhr.response)
        this.setState({ users })
      }
    })
    xhr.send(null)
  }
```

This is old method, better to use fetch() or Axios, but this also works fine.

That's all!



