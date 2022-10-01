---
title: "Typescript basics - generic and uility types"
excerpt: "Typescript basics - generic and uility types"
last_modified_at: 2022-09-29T10:27:01-05:00
categories:
  - JavaScript
  - TypeScript
tags: 
  - basics
---

<!-- short introduction -->
## Typescript basics - generic and uility types



Generic types:

{% include code-header.html %}
```ts
// basic examples
const names: Array<string> = [];
// map key value
const mapObj = new Map<string,number>();
// unique set
const setObj = new Set<number>();

//function that takes and returns the same type
function getAndReturnTheSameType<T>(item:T){
  return item;
}
// class thet use Generic type

type FrontendFrameworks = 'React' | 'Angular';
type BackendFrameworks = 'Node.js'| 'JavaSpring';

type Developer = {
  firstName: string,
  lastName: string,
  pesel: number,
}

type FrontendDeveloper = {
  frontendFramework: FrontendFrameworks;
} & Developer; // this will add all properties from Developer type

type BackendDeveloper = {
  backendFramework: BackendFrameworks;
} & Developer;

// now Generic interface that will be compatibile with both developers type
interface DevelopersTeamInterface<T> {
  team: Array<T>;
  addDeveloper(newDeveloper: T): void;
  getDeveloper(pesel: number): T | null;
  removeDeveloper(pesel: number): void;
  updateDeveloper(pesel: number, newData: T): void;
}

// now lets use it in class, that can use FrontendDeveloper type or BackendDeveloper type:
class DevelopersTeam<T extends FrontendDeveloper | BackendDeveloper> implements DevelopersTeamInterface<T> {
team: Array<T> = [];
addDeveloper(newDeveloper: T){
  this.team.push(newDeveloper);
}
getDeveloper(pesel: number){
  return this.team.find(item => item.pesel === pesel) ?? null; // if not find (?? -> undefined) then return null
}
removeDeveloper(pesel: number){
  this.team = this.team.filter(item => item.pesel!==pesel);
}
updateDeveloper(pesel:number, newData: T){
  this.team = this.team.map(item => {
    if (item.pesel === pesel){
      return {...item, ...newData}
    }
    return item;
  })
}
}

// now use our class - the same class will use two different types (and only these two):
const frontendDevs = new DevelopersTeam<FrontendDeveloper>();
frontendDevs.addDeveloper({
  firstName: 'John',
  lastName:'Doe',
  pesel:234567890,
  frontendFramework:'React'
})

const backendDevs = new DevelopersTeam<BackendDeveloper>();
backendDevs.addDeveloper({
  firstName: 'Alice',
  lastName:'Cooper',
  pesel:3344552234,
  backendFramework:'Node.js'
})

```

That's all!






