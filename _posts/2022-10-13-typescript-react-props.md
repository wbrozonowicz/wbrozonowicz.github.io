---
title: "Typescript & React - props"
excerpt: "Typescript & React - props"
last_modified_at: 2022-10-12T10:27:01-05:00
categories:
  - JavaScript
  - TypeScript
  - React
tags: 
  - basics
---

<!-- short introduction -->
## Typescript  & React - props


- in class components:

{% include code-header.html %}
```ts
import {Component} from 'react';

type ClassExampleProps = {
  startValue: number;
}
class ClassExample extends Component<ClassExampleProps>{

  render(){
    return <p>{this.props.startValue}</p>
  }
}

export default ClassExample;

// usage

import ClassExample from "./ClassExample";

export default function App(){
  return(
    <div><ClassExample startValue={44}></div>
  )
}

```


- in functional components:

{% include code-header.html %}
```ts
type FunctionComponentExampleProps = {
  startValue: number;
}
const FunctionComponentExample = (props: FunctionComponentExampleProps) => {
return <p>{props.startValue}</p>;
}

export default FunctionComponentExample;

// usage

import FunctionComponentExample from "./FunctionComponentExample";

export default function App(){
  return(
    <div><FunctionComponentExample startValue={77}></div>
  )
}

```

- another version with default value (when use FC We can optionally have also acces to children, and default props, but easier is just decrale props = value)

{% include code-header.html %}
```ts
import { FC } from "react";

type FunctionComponentExampleProps = {
  readonly startValue?: number;  // optional ("?")
}
const FunctionComponentExample: FC<FunctionComponentExampleProps> = ({ startValue = 111 }) => {
return <p>{startValue}</p>;
}

export default FunctionComponentExample;

// usage

import FunctionComponentExample from "./FunctionComponentExample";

export default function App(){
  return(
    <div><FunctionComponentExample></div> // don't have to pass props, by default is 111
  )
}
```



That's all!






