---
title: "Pass all object properties in props in REACT - simple way"
excerpt: "Pass all object properties in props in REACT"
last_modified_at: 2022-08-10T10:27:01-05:00
categories:
  - JavaScript
  - React
tags: 
  - basics
---

<!-- short introduction -->
## Pass all object properties in props in REACT

Let's assume, that We have object, that has many properties. We want to pass all of them in props to another component. For example, our objects are in array as below:

{% include code-header.html %}
```js
const myObjects = [
    {
        id: 1,
        property1: 45,
        anotherProperty: "Something",
        differentProperty: false
    },
    {
       id: 2,
        property1: 32,
        anotherProperty: "Something",
        differentProperty: false
    },
    {
     id: 3,
        property1: 67,
        anotherProperty: "Something",
        differentProperty: false
    }
]

```
We can of course individually pass all properties like below to render them:

{% include code-header.html %}
```js
  const myObjectsToRender = myObjects.map(myObject => (
        <MyObjectComponent key={myObject.id} property1={myObject.property1} anotherProperty={myObject.anotherProperty} differentProperty={myObject.differentProperty}/>
    ))
```

And then use it in MyObjectComponent:
{% include code-header.html %}
```js
  import React from 'react';

const MyObjectComponent = (props) => {
    return (
        <article>
            <h3>{props.property1}</h3>
            <span>{props.anotherProperty}</span>
            <p >{props.differentProperty}</p>
        </article>
    );
}
export default MyObjectComponent
```

It's OK, but there's also very coll way to do this, as below:
{% include code-header.html %}
```js
  const myObjectsToRender = myObjects.map(myObject => (
        <MyObjectComponent key={myObject.id} {...myObject}/>
    ))
```
This way, by using "{...myObject}" We will pass all properties of myObject to props!

Now We can use it (instead of props, We can define which We will ne using by "{ property1, anotherProperty, differentProperty }"):
{% include code-header.html %}
```js
import React from 'react';

const MyObjectComponent = ({ property1, anotherProperty, differentProperty }) => {
    return (
        <article>
            <h3>{property1}</h3>
            <span>{anotherProperty}</span>
            <p>{differentProperty}</p>
        </article>
    );
}
export default MyObjectComponent
```

That's all!



