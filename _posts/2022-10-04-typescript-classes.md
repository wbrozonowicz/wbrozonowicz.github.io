---
title: "Typescript basics - classes"
excerpt: "Typescript basics - classes"
last_modified_at: 2022-09-29T10:27:01-05:00
categories:
  - JavaScript
  - TypeScript
tags: 
  - basics
---

<!-- short introduction -->
## Typescript basics - classes



- Classes:

{% include code-header.html %}
```ts
// best way to declare class:
class Person {
constructor(
  public age: number, // public is accessible inside and outside class
  private firstName: string,  // private is only accessible within class
  readonly private lastName: string, // readonly is only possible to set in constructor
  protected public country: string, // protected is accessible only within class and its subclasses
  )

public changeName(newName: string){ // public is by default
this.firstName = newName;
}

private logName(){ // private method
console.log(this.lastName);
}

// getter and setter
get age() {
  return this.age;
}

set age(newAge: number) {
if (newAge < 0){
this.age = 0;
} else {
  this.age = newAge;
}
}

}

class Employee extends Person {
  constructor( 
   age: number, firstName: string, lastName: string, country: string, ){
    super();
   }
}

const newPerson = new Person(22, 'John', 'Doe','USA');
const newEmployee = new Employee(32, 'John', 'Boe','Canada');

// abstract class -> not possible make instance of it, it is only definition of class structure
abstract class Human {
abstract age: number;
abstract firstName: string;
abstract lastName: string;

abstract getAge(): number;
}

class Man extends Human {
  constructor(
    public age: number,
    public firstName: string,
    public lastName: string,
  ) {
    super();
  }

  public getAge(){
    return this.age;
  }
}

// interface version
interface Human {
 age: number;
 firstName: string;
 lastName: string;

 getAge: () => number;
}

class Man implements Human {
  constructor(
    public age: number,
    public firstName: string,
    public lastName: string,
  )  {}
  

  public getAge(){
    return this.age;
  }
}

```

Summary:
- for simple cases use interface
- for more complex cases use abstract class

That's all!






