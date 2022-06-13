---
title: "Use setInterval() in React - start and stop"
excerpt: "Usage of setInterval() in React in proper way"
last_modified_at: 2022-06-13T10:27:01-05:00
categories:
  - JavaScript
  - React
tags: 
  - basics
---

<!-- short introduction -->
## Usage of setInterval() in React in proper way

To use properly setInterval() in React, follow below tips:

1. define function that will be execute in some interval

{% include code-header.html %}
```js
	  functionToRunEverySecond = () => {
    //  something happen here
  }
```

2. probably, this function will get some data (from API) and setState. The best way is to do this in ComponentDidMount (function that is execute only once, after first render).
We need id of this interval, to clear it (stop) later, so We will create parameter name "idOfInterval" that will receive id of this specific interval. 
SetInterval takes as a first parameter function to execute in some interval of time, and second parameter time in miliseconds. SetInterval returns id of started interval. 
Let's take a loook:

{% include code-header.html %}
```js
componentDidMount() {
    this.idOfInterval = setInterval(this.functionToRunEverySecond, 1000)
  }
```

3. Now our function is running, but the good practice is to stop it (clear) when it is not needed. The best way is to do this in componentWillUnmount function:

{% include code-header.html %}
```js
componentWillUnmount() {
    clearInterval(this.idOfInterval)
  }
```

In different scenario, We can start our interval f.e. by clicking button. Then just use setInterval in handleClick function assign to button (and the same procedure for clear interval..).

That's all!



