---
title: "Typescript basics - decorators"
excerpt: "Typescript basics - decorators"
last_modified_at: 2022-10-12T10:27:01-05:00
categories:
  - JavaScript
  - TypeScript
tags: 
  - basics
---

<!-- short introduction -->
## Typescript basics - decorators


Decorators can be used to add some functionality by annotate class or method. 

For example:
- decorator @addID will ad id property to class
- decorator @minVal will check if age is more or equal to 0

{% include code-header.html %}
```ts

// decorators declaration
function addID<T extends { new (...args: any[]): {}}>(classToDecorate: T) {
  returb class extends classToDecorate {
    id: number;
    constructor(...args: any[]){
      super(...args);
      this.id = Math.random();
    }
  }
}

function minValue(value: number) {
  return function(    
    target: Object,
    property: string,
    descriptor: PropertyDescriptor){
    const inputFunction = descriptor.value; // decorated original function
    const returnedFunction = function(this: Object, ...any[]){
      console.log("before function execution...);
      if (args.some((ageVal) => ageVal < value )){
        return console.log("Age can not be smaller than 0!");
      }
      inputFunction.apply(this, args);
      console.log("after function execution...);
    }
  }
}


// class to decorate with id
@addID
class Person {
constructor(
  public age: number, 
  private firstName: string,  
  private lastName: string, 
  public country: string, 
  )

public changeName(newName: string){ 
this.firstName = newName;
}

// decorated method
@minValue(0)
public changeAge(newAge: number){ 
this.age = newAge;
}

private logName(){ 
console.log(this.lastName);
}

}


// usage
const person = new Person(32,'Joe', 'Doe', 'USA'); // this object will have random id added automatically by decorator

person.setAge(-3) // will not set age and log info


```


That's all!






