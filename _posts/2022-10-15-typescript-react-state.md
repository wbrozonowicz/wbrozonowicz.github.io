---
title: "Typescript & React - state"
excerpt: "Typescript & React - state"
last_modified_at: 2022-10-12T10:27:01-05:00
categories:
  - JavaScript
  - TypeScript
  - React
tags: 
  - basics
---

<!-- short introduction -->
## Typescript  & React - state


- in class components:

{% include code-header.html %}
```ts
import {Component} from 'react';

type ClassExampleProps = {
 readonly startValue: number;
}

type ClassExampleState = {
  readonly counter: number;
  readonly counter2: number;
}


class ClassExample extends Component<ClassExampleProps, ClassExampleState>{

  readonly state: ClassExampleState = {
    counter: this.props.startValue,
    counter2: -1,
  }

  render(){
    const { counter } = this.state;

    return
    <>
     <p>{counter}</p>
     <button onClick={() =>this.incrementCounter()}>add</button>
     </>
  }
}

incrementCounter() {
  this.setState((prevState, prevProps) => (
    { counter: prevState.counter+1 }
  ));
}

// example of componentDidUpdate with types
componentDidUpdate(prevProps:ClassExampleProps, prevState: ClassExampleState){
  if (prevProps.startValue !== this.props.startValue){
    return null; // do something here
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
import { useState, Dispatch, SetStateAction } from 'react;

type FunctionComponentExampleProps = {
  startValue?: number | string;
}

// example of inner component that get function from our component
type ButtonInnerComponentProps = {
onClickAction: Dispatch<SetStateAction<number>>;
}

const ButtonInnerComponent = ({onClickAction}: ButtonInnerComponentProps) => (
  <button onClick={() => onClickAction(prev => prev+1)}>add</button>
);

const FunctionComponentExample = ({startValue = '33'}: FunctionComponentExampleProps) => {
  const [counter, setCounter] = useState<number>(Number(startValue))
 
  const incrementValue = () => setCounter((prevState) => prevState + 1);

return (
  <>
   <p>{counter}</p>;
   <button onClick={incrementValue}>add</button>
   <ButtonInnerComponent onClickAction={setCounter}/> //  pass function to inner component
   </>
);
};


export default FunctionComponentExample;

// usage

import FunctionComponentExample from "./FunctionComponentExample";

export default function App(){
  return(
    <div><FunctionComponentExample startValue={77}></div>
  )
}

```

That's all!






