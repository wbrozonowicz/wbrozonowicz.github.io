---
title: "Typescript basics - basic types"
excerpt: "Typescript basics - basic types"
last_modified_at: 2022-09-29T10:27:01-05:00
categories:
  - JavaScript
  - TypeScript
tags: 
  - basics
---

<!-- short introduction -->
## Typescript basics - basic types

Typescript is "addition" to JavaScript that makes it "typed".

Basic types:

{% include code-header.html %}
```ts
let isBoolean = true; // equivalent to: let isBoolean: boolean = true
let someNumber = 10; // equivalent to: let someNumber: number = 10
let someText = 'text'; // equivalent to: let someText: string = 'text
let textOrNull : string | null; // union of two types
// Array:
const someArray: Array<string> = []; 
// or
const someArray: string[] = []
let tupleExample: [number, string] = [32, 'name'] // tuple is array with defined number of elements (like useState in React)
enum UserTypes {
ADMIN,
MODERATOR,
USER = 10
DEMOUSER
}
// each enum has incremental id, if not declared then started with 0 or previous id +1 f.e. ADMIN = 0, MODERATOR = 1, USER = 10, DEMOUSER=11 etc.
// use enum:
let myTypeOFUser = UserTypes.ADMIN;
//const String
const ConstStringsExample = {
  EVENT_CLICK: 'click',
  EVENT_CHANGE: 'change'
} as const;

let unknownExample: unknown; // type that could be different 
if (typeof unknownExample === 'string'){ stringArray.push(unknownExample) }
let badExample: any; // any is 'anything' - don't use it in TS unless You have realy good reason...
function showLog(): void {console.log('log...')}; // type void for function yhat doesn't return any value (type)
// there is also type "never" but it is for very rare and special cases...
```



That's all!






