---
title: "JavaScript arrays - delete element and change element in React app"
excerpt: "Working with Js arrays in React"
last_modified_at: 2022-07-11T10:27:01-05:00
categories:
  - JavaScript
tags: 
  - basics
---

<!-- short introduction -->
## JavaScript arrays - how to delete or chenge element

We have in our React state array of objects as below:

{% include code-header.html %}
```js
state = {
    elements: [
      {
        id: 0,
        text: 'text1',
        date: '2022-02-15',
        finished: false,
        endDate: null
      },
      { id: 1, text: "text2", date: '2022-11-12', finished: false, endDate: null },
      { id: 2, text: "text3", date: '2022-09-11', finished: false, endDate: null },
    ]
  }
```

To work with this array, We will use two methods: one for deleting one element (deleteElement), second for changing one element (changeElement)

1] deleting element with some id (always get id from click event!)
- first We will create copy of array from state (We use "..." operator for this)
- next step is to get index of element with specific id (index could be different than id) and assign it to const "index". Method findIndex iterates on each element of array and if condition is true, then return index. If condition is never true then it will return -1.
- third step is to remove element from array, with splice method (first argument is index, second arguments - how many elements remove)
- the last step is to set new state with our copy of array, that We modified in previous steps

{% include code-header.html %}
```js
	deleteElement = (id) => {
    const elements = [...this.state.elements];
    const index = elements.findIndex(element => element.id === id);
    element.splice(index, 1);
    this.setState({
      elements
    })
  }
```

Alternatively We can use filter method as below. The difference is that filter method returns new array ith elements that match specified condition. So When our condition is that id of element should be different than id of clicked element, We will get array without it.

{% include code-header.html %}
```js
	deleteElement = (id) => {
    let elements = [...this.state.elements];
    elements = elements.filter(element => element.id !== id)
    this.setState({
      elements
    })
  }
```

2] changing element with specified id. We want to change finished to true and endDate to time of click.
This time to make copy of array from state We will use Array.from() method. To get element to change, We will use foreach() method. This method not return new array, so We can use it on const elements. Let's see:

{% include code-header.html %}
```js
	changeElement = (id) => {
    const elements = Array.from(this.state.elements);
    elements.forEach(element => {
      if (element.id === id) {
        element.finished = false;
        element.endDate = new Date().getTime()
      }
    })
    this.setState({
      elements
    })
  }
```

That's all!



