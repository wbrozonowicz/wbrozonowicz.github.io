---
title: "JavaScript - About THIS! bind, call, apply"
excerpt: "What is THIS and how bind, call, apply methods work"
last_modified_at: 2022-10-12T10:27:01-05:00
categories:
  - JavaScript
  - TypeScript

tags: 
  - basics
---

<!-- short introduction -->
## JavaScript - About THIS!

"This" is owner object of function. So if we create function in global namespace f.e.:
```js
function ExampleFunction(){
  console.log(`my THIS is:${this}`)
}
```

...and now run this function directly, then we will see that This is owner, in web browser it is window.
If function is defined for specific object - the owner (this object) will be "this"

```js
ExampleFunction();
```

But functions also can be object in JS and for example:
```js
let functionAsObject = new ExampleFunction();
```

In this case "This" will be different: object function "ExampleFunction" will be owner and "This".

So "This" depends on context!
What is problem with "this" in JS?
- Sometimes reference (context) is changed and "This" is different, than We think... (f.e. in promises or DOM elements in addEventListener) 

Solution: use bind, call or apply method to define context, or arrow function.
Arrow function will always takes as context upper scope so it is easy to use (easier to identify what "This" is).

Some rules:

- arrow function —  "this" is from surrunding scope
- object or function as object, with "new" keyword — "this" is creted object
- call/apply/bind — 'this' is object passed to these methods as first argument
- when function is run on object f.e. object.run() — "this" is object on which we run function
- default — undefined in strict mode, global object in different case


- bind will permanently assign function to this of different object!
- call and apply will assign function to this of different object only once, during execute
- call takes first argument - object to bind, and next other arguments of function (as in its signature)
- apply takes first argument - object to bind, and next array of arguments of function (as in its signature)
- after bind, call and apply will not work

- Example code:

{% include code-header.html %}
```ts
class FirstClass {
    name: string = 'class 1';
    rating: number = 100;
    showMyName() {
        console.log(this.name);
    }
}

class SecondClass {
    name: string = 'class 2';
    rating: number;
    showMyName(){
        console.log(this.name);
    }
    setMyRating(ratingIn: number){
        this.rating = ratingIn;
        console.log(`rating set for ${this.name} = ${this.rating}`);
    }

}

const object1 = new FirstClass();
console.log("run function showMyName of object1:");
object1.showMyName();

const object2 = new SecondClass();
console.log("run function showMyName of object2:");
object2.showMyName();

// bind: allows to permanently change "this" of function from one object to another, can have many additional arguments
console.log("binding to object 1")
object2.showMyName = object2.showMyName.bind(object1);
console.log("run function showMyName of object2 after binding to object1:");
object2.showMyName();

// call and apply: changes "this" only once - during execute f.e.
// call takes many arguments, apply takes array of arguments
object2.setMyRating(4); // set rating for object2

//call example:
console.log("call for object1")
object2.setMyRating.call(object1,5); // set rating for object1 with value 5
object2.setMyRating(6); // set rating again for object2 with value 6

//aplly example:
console.log("apply for object1")
object2.setMyRating.apply(object1,[7]); // set rating for object1 with value 57
object2.setMyRating(9); // set rating again for object2 with value 9

// now bind - permanent change of binding this, after that next call and apply will not change "this"!
console.log("binding setMyRating to object1")
object2.setMyRating=object2.setMyRating.bind(object1);

object2.setMyRating(55);  // set rating for object1 with value 55
object2.setMyRating(15); // set rating for object1 with value 15
object2.setMyRating.call(object2,7); // still set rating for object1! with value 7

```

Results in console:

- run function showMyName of object1:
- class 1
- run function showMyName of object2:
- class 2
- binding to object 1
- run function showMyName of object2 after binding to object1:
- class 1
- rating set for class 2 = 4
- call for object1
- rating set for class 1 = 5
- rating set for class 2 = 6
- apply for object1
- rating set for class 1 = 7
- rating set for class 2 = 9
- binding setMyRating to object1
- rating set for class 1 = 55
- rating set for class 1 = 15
- rating set for class 1 = 7


That's all!





