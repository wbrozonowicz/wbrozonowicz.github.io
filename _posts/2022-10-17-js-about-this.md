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






