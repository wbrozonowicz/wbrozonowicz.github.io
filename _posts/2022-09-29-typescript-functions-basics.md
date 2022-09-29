---
title: "Typescript basics - basic functions"
excerpt: "Typescript basics - basic functions"
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

Basic functions:

{% include code-header.html %}
```ts
function add(firstNum: number, secondNum: number): number {
  return firstNum+secondNum;
const res = add(3,4); // will return 7
}
// equivalent - TS will define retuned type based on parameters :
function add(firstNum: number, secondNum: number) {
  return firstNum+secondNum;
}
// default values, also will make them optional, when not passed TS wil take defaults
function add(firstNum: number = 0, secondNum: number = 0) {
  return firstNum+secondNum;
}
// equivalent:
function add(firstNum = 0, secondNum = 0) {
  return firstNum+secondNum;
}
// with use of union types:
function add(firstNum: number | string = 0, secondNum: number | string = 0) {
  return Number(firstNum)+Number(secondNum);
}
const res = add(3,'4'); // will return 7
// typed array function
type FuncAddType  = (firstNum?: number | string, secondNum?: number | string) => number // definition of function type, that takes two optional ("?") parameters and returns number
const addFunc: FuncAddType = (firstNum = 0, secondNum = 0) => {
  const a = typeof firstNum === 'number' ? firstNum : Number(firstNum);
  const b = typeof secondNum === 'number' ? secondNum : Number(secondNum);
  return a + b;
}
```

That's all!






