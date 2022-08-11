---
title: "Redirect in React Router"
excerpt: "Redirect in React Router"
last_modified_at: 2022-08-11T10:27:01-05:00
categories:
  - JavaScript
  - React
tags: 
  - basics
---

<!-- short introduction -->
## Redirect in React Router

For example: We have AdminPageComponent, but if user is not logged, We want to redirect him to component LoginPageComponent. How to do this?
We will use for it "AdminComponent" and "Redirect" from React router

Below simple example:

{% include code-header.html %}
```js
import React from 'react';
import { Route, Redirect } from 'react-router-dom';

const isUserLoggedIn = false;

const AdminComponent = () => {
    return (
        <Route render={() => (
            isUserLoggedIn ? <AdminPageComponent/>: <Redirect to="/login" />
        )} />
    );
}
export default AdminComponent;
```
Notes:
- We will show AdminPageComponent only when isUserLoggedIn = true
- if it is false, We will change path to "/login". We have to define in router what will be rendered on this path, for example:


{% include code-header.html %}
```js
 import React from 'react';
import { Route, Switch } from 'react-router-dom';

import HomePageComponent from '../pages/HomePageComponent';
import ProductPageComponent from '../pages/ProductPageComponent';
import ProductListPageComponent from '../pages/ProductListPageComponent';
import ContactPageComponent from '../pages/ContactPageComponent';
import AdminComponent from '../pages/AdminComponent';
import ErrorPageComponent from '../pages/ErrorPageComponent';
import LoginPageComponent from '../pages/LoginPageComponent';

const Page = () => {
  return (
    <>
      <Switch>
        <Route path="/" exact component={HomePageComponent} />
        <Route path="/products" component={ProductListPageComponent} />
        <Route path="/product/:id" component={ProductPageComponent} />
        <Route path="/contact" component={ContactPageComponent} />
        <Route path="/admin" component={AdminComponent} />
        <Route path="/login" component={LoginPageComponent} />
        <Route component={ErrorPageComponent} />
      </Switch>
    </>
  );
}

export default Page;
```

That's all!



