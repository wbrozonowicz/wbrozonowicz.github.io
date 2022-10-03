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



- Generic types:

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

- Utility Types:

{% include code-header.html %}
```ts
// Partial -> only some of properties are rquired f.e.:
type Person = {
  id: number;
  firstName: string;
  lastName: string;
}

const newMan = {
  id:1,
  firstName: 'Adam',
  lastName: 'First',
}

const editedManData = {
  lastName: 'Second',
}

function updatePersonData(obj: Person, objData: Partial<Person>){
return {...obj, ...objData}
}

updatePersonData(newMan, editedManData); // second argument is Partial so doean't have to have all parameters from Person, but only one that is the same f.e. lastName

// Required -> makes all parameters (also optional) required f.e. 

type Employee = {
  id: number;
  lastName: string;
  position?: string; // optional
}

function addWorker(worker: Required<Employee>){
  workers.push(worker);
}

// ReadOnly -> makes all properties 'read only' - after set can not be updated

const someMan: ReadOnly<Employee> = {
  id:1,
  lastName: 'Doe',
  position: 'manager'
}

someMan.lastName = 'EditedName' // not possible!!!
//ReadOnly also can be used in function to be sure that it will not edit object..

// Record -> makes kind of 'key - value' pair object f.e.
type PossibleKeys = 'pl' | 'de' | 'en';
type LanguageData = {
  nameOfLanguage: string,
  difficultyLevel: string,
}
type Lang = Record<PossibleKeys, LanguageData>

const myLang: Lang = {
  pl: {nameOfLanguage: 'polish', difficultyLevel: 'hard'},
  en: {nameOfLanguage: 'english', difficultyLevel: 'easy'},
  de: {nameOfLanguage: 'german', difficultyLevel: 'moderate'},  
};

// Pick -> makes only picked properties available in fuction f.e.:
type Human = {
  firstName: string;
  lastName: string;
  age: number;
}

functionUpdateHumanNamePick(humanToUpdate:Pick<Human, 'firstName' | 'lastName'>, newName:string, newLastName:string){
humanToUpdate.firstName = newName;
humanToUpdate.lastName = newLastName;
// age is not editable in this function (it is not specify in Pick<>)
}

// Omit -> opposite to Pick f.e.
functionUpdateHumanNameOmit(humanToUpdate:Omit<Human, 'age'>, newName:string, newLastName:string){
humanToUpdate.firstName = newName;
humanToUpdate.lastName = newLastName;
// age is not editable in this function (it is specify in Omit<>)
}

// Exclude -> get from union without specified members f.e.:
type Sport = 'BasketBall' | 'Football' | 'Ski jumping';
type SummerSport = Exclude<Sport, 'Ski jumping'>;
const myFavSport: SummerSport = 'Ski jumping' // not allowed - Ski jumping was excluded from Sport type!

//  Extract -> get from union only specified members f.e.:
type SummerSport = Extract<Sport, 'BasketBall' | 'Football' >;
const myFavSport: SummerSport = 'Ski jumping' // not allowed - Ski jumping was not extracted fromSport type!

// NonNullable -> makes type without null & undefined
type UserDescription = string | null | undefined;
type TypeUserDescriptionNotNullable = NonNullable<UserDescription>; // now only string is OK for this type

// Parameters -> makes type with sepecified parameters f.e.:
function exampleFunction(id:number, text:string, value: number){
  console.log(`id=${id} text= ${text} value=${value}`);
}

type NeededParams = Parameters<typeof exampleFunction>;
const ParemetersForFunction: NeededParams = [1, 'some text', 999]; 
exampleFunction(...ParemetersForFunction); // here We use specified parameters in specified order: id, text and value

// ConstructorParameters -> takes parameter form class constructor f.e.:
class User{
  constructor(
    public age: number,
    public nameOfUser: string,
  ) {}
}

type UserClassParams = ConstructorParameters<typeof User>;
const userParams: UserClassParams = [24, 'Adam']; // here We use specified parameters from constructor of User class
new User(...userParams);

// ReturnType -> type that is returned from some function
type Dude = {
  age: number,
  firstName:string,
  lastName:string,
}

function getAge(dude: Dude){
  return dude.age;
}

const dudeAge: ReturnType<typeof getAge> = 32; // const dudeAge is type of number because it is type rturned from getAge function 

```

That's all!






