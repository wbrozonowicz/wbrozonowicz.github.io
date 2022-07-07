---
title: "Object destructurization in React - simple example "
excerpt: "Object destructurization in React"
last_modified_at: 2022-07-07T10:27:01-05:00
categories:
  - JavaScript
  - React
tags: 
  - basics
---

<!-- short introduction -->
## Object destructurization in React - simple example 

When We have complex object in React (for example in State), when We pass it to another component (in props) than We have to write full name of it like below, or We can use destructurization of this object.

Object in state - array "some_objects":


{% include code-header.html %}
```js
state = {
 some_objects: [
      {
        id: 0,
        name: 'Object no.1',
        date: '2022-08-15',
        status: 'Forbidden',
        active: true,
      },
}
```

We pass this object to another component by props:

```js
  <ObjectList objects={this.state.some_objects}/>
```

And further to component that represent only one element of array (with index=0) (usually by map() function, below just by index to simplify example):
```js
  <Object object={this.state.some_objects[0]}/>
```

And now in component Object We have to each time write "props.object.name", "props.object.date" etc.
To make this shorter We can use destructurization and just get all elements We need from this object like below in just one line of code:  


```js
  const { name, date, id, status, active } = props.object;
```

Now We can use this short names like "name", "date", "id" etc in our code:

```js
if (active) {
    return (
      <div>
        <p>
          Object with name <strong>{name}</strong> is active
        </p>

      </div>
    );
  } else {
    return (
      <div>
        <p>
         Object with name {name} has status <h3>{status}</h3> is not active
        </p>
      </div>
    )
  }
```


That's all!



