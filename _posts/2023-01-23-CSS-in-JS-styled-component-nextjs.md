---
title: "Next.js - styled component CSS-in-JS"
excerpt: "CSS styling with Next.js"
last_modified_at: 2023-01-22T10:27:01-05:00
categories:
  - CSS
  - Next.js

tags: 
  - basics
---

<!-- short introduction -->
## CSS-in-JS Styled component in Next.js!

Next.js -> use hermetic css style in component:
ONLY FOR CLIENT SIDE COMPONENTS!

install library f.e.:

{% include code-header.html %}
```
npm install styled-components
```

Use in component:

{% include code-header.html %}
```js
import React from 'react';
import styled from 'styled-components';

const Button = styled.button`
  background-color: ${props => props.primary ? 'blue' : 'white'};
  color: ${props => props.primary ? 'white' : 'black'};
  border: 2px solid blue;
  padding: 10px 20px;
  font-size: 16px;
  cursor: pointer;

  &:hover {
    background-color: ${props => props.primary ? 'darkblue' : 'lightgray'};
  }
`;

const App = () => {
  return (
    <div>
      <Button primary>Primary Button</Button>
      <Button>Secondary Button</Button>
    </div>
  );
};

export default App;
```

Other libraries:
 -check docs:

 https://nextjs.org/docs/app/building-your-application/styling/css-in-js

 


