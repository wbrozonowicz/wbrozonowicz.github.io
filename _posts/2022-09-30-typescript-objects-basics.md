---
title: "Typescript basics - basic objects, interface & type"
excerpt: "Typescript basics - basic objects"
last_modified_at: 2022-09-29T10:27:01-05:00
categories:
  - JavaScript
  - TypeScript
tags: 
  - basics
---

<!-- short introduction -->
## Typescript basics - basic objects interface & type

Typescript is "addition" to JavaScript that makes it "typed".

Basic objects:

{% include code-header.html %}
```ts
interface User {
  age: number;
  names: {
    firstName: string;
    lastName: string;
  }
}

const user: User = {
  age: 22,
  names: {
    firstName: 'Joe',
    lastName: 'Doe',
  }
}

// nested
interface Names: {
    firstName: string;
    lastName: string;
  }

interface User {
  age: number;
  names: Names;
}

const user: User = {
  age: 22,
  names: {
    firstName: 'Joe',
    lastName: 'Doe',
  }
}

// with type instead of interface
type User  = {
  age: number,
  names: {
    firstName: strin,
    lastName: string,
  }
}

const user: User = {
  age: 22,
  names: {
    firstName: 'Joe',
    lastName: 'Doe',
  }
}

// two types, one without one property, used in one array
type User  = {
  age: number,
  names: Names,
}

type SimpleUser  = {
  names: Names,
}

type ExperiencedUser  = {
  experience: number,
  names: Names,
}

// function that shows all properties
function showUserDetails(user: User | SimpleUser): string {
  return `${user.names.firstName} ${user.names.lastName} age= ${user.age}`;
}

// function that shows only names -> we can type "smaller" type
function showUserNames(user: SimpleUser): string {
  return `${user.names.firstName} ${user.names.lastName}`;
}

// function that returns only common properties
function showCommonProperties(user: User | SimpleUser): string {
  return `${user.names.firstName} ${user.names.lastName}`; // We have acces here only to common properties, age is not available here
}

const usersList : (User | SimpleUser)[] = [];
// if we Have user: User and simpleUser: SimpleUser
usersList.push(user);
usersList.push(simpleUser);

// log all properties -> age exists only in User
usersList.forEach(item => {
  if ('age' in item){   // check if property age exists in object from list
    console.log(showUserDetails(item));
  } else {
    console.log(showUserDetails({...item, age: 0})); // if age not exists, let's add it with value = 0
  }
})

// log only names -> they are present in both types
usersList.forEach(item => {
    console.log(showUserNames(item)); // ts is OK that functions takes SimpleUser and we have User (both have names property that function use)
})

// type of function
type UserFunctionExample = (object: SimpleUser) => string;
let myFunction : UserFunctionExample = showUserNames;

// object that has properties from two types (User has age but not experience, ExeriencedUser has experience, but not age, We want to have object with both)
const AllPropertiesUser : User & ExperiencedUser = {
 age: 22,
 experience: 4,
  names: {
    firstName: 'Alice',
    lastName: 'Preety',
  },
}

// type with limited options f.e. number but only 1 to 4
type Level = 1 | 2 | 3 | 4;
let level: Level = 3; // possible
level = 7; // not possible

```

- type can be declared only once
- interface can be declared many times, than it is "joined" f.e.

{% include code-header.html %}
```ts
interface Point {
  x: number,
  y: number,
}

interface Point {
  z: number,
}

// now Point needs all 3 properties:
const point: Point = {
  x:2,
  y:4,
  z:9,
}

// another way is to extends point with extra property z:
interface Point3D extends Point{
  z:number
}

cont point3D: Point3D = {
  x:4,
  y:9,
  z:12,
}

interface Description {
  desription: string
}

// We can also extends two interfaces and add new property:
interface PointWithDescription extends Point, Description{
  z?:number // optional property - use ?
}

cont pointWithDescription: PointWithDescription = {
  x:4,
  y:9,
  desription: 'some text',
}
```

- read only objects ore object with read only property

{% include code-header.html %}
```ts
// read only object
const fixedObject = {
  p1: 'a',
  p2: 'b'
} as const;
// now We can not change p1 and p2 for this object

// object with read only property
interface ObjectWithReadOnlyProperty {
  readonly p1: string,
  p2: string,
}
const objectWithReadOnlyProperty = {
  p1: 'a',
  p2: 'b'
}

objectWithReadOnlyProperty.p1 = 'xyz' // not possibe -> p1 is readonly
objectWithReadOnlyProperty.p2 = 'c' // OK -> p2 is editable

```

- interface can be implemented by class (then interface is definition of class structure), for more info -> check post about TS classes

Summary:
- when not need to extend -> just use type
- when need to extend -> use interface

That's all!






