---
title: "Dynamic subpages in React router"
excerpt: "Dynamic subpages in React router"
last_modified_at: 2022-08-13T10:27:01-05:00
categories:
  - JavaScript
  - React
tags: 
  - basics
---

<!-- short introduction -->
## Dynamically geerated subpages in React

For example: We have many subpages to show (some articles from database). Each article should be render on different page, with dynamically generated route (f.e. article/1, article/2 etc).

We will create one Component: "Article.js" and render it on dynamically generated route, with some data.

Below simple example:

{% include code-header.html %}
```js
import React from 'react';
import { Link } from 'react-router-dom';
import '../styles/ArticleListPage.css';

// data from db or API
const articles = [{id:1, text: "abc"}, {id:2, text: "123"}, {id:3, text: "xxx"}]; 

const ArticleListPageComponent = () => {

  const listOfArticles = articles.map(article => (
    <li key={article.id}>
      <Link to={`/article/${article.id}`}>Article no.: {article.id}</Link>
    </li>
  ))

  return (
    <div className="articles">
      <h2>Our articles</h2>
      <div>
        <ul>
          {listOfArticles}
        </ul>
      </div>
    </div>
  );
}

export default ArticleListPageComponent;
```
Notes:
- This component desn't have inside Article component (with data) - it only has Links to path "Link to={`/article/${article.id}`}"

Simple css file ArticleListPage.css for this component below:

{% include code-header.html %}
```js
.articles h2 {
  margin-bottom: 20px;
}

.products ul {
  list-style: square;
  padding-left: 30px;
}
.articles li {
  margin-bottom: 20px;
}
.articles a {
  text-decoration: none;
  color: #333;
}

.articles a:hover {
  text-decoration: underline;
  color: black;
}
```

Now We have to make route for it, to make it dynamically with many different ids, We will use "/article/:id" statement in path:

```js
import React from 'react';
import { Route, Switch } from 'react-router-dom';

import HomePageComponent from '../pages/HomePageComponent';
import ArticlePageComponent from '../pages/ArticlePageComponent';
import ProductListPageComponent from '../pages/ArticleListPageComponent';
import ContactPageComponent from '../pages/ContactPageComponent';
import AdminPageComponent from '../pages/AdminPageComponent';
import ErrorPageComponent from '../pages/ErrorPageComponent';
import LoginPageComponent from '../pages/LoginPageComponent';

const Page = () => {
  return (
    <>
      <Switch>
        <Route path="/" exact component={HomePageComponent} />
        <Route path="/articles" component={ArticleListPageComponent} />
        <Route path="/article/:id" component={ArticlePageComponent} />
        <Route path="/contact" component={ContactPageComponent} />
        <Route path="/admin" component={AdminPageComponent} />
        <Route path="/login" component={LoginPageComponent} />
        <Route component={ErrorPageComponent} />
      </Switch>
    </>
  );
}

export default PageComponent;
```

Great, now let's create page to show each Article (article will be in seperate component):

```js
import React from 'react';
import { Link } from 'react-router-dom';
import Article from '../components/Article'


const ArticlePagComponent = ({ match }) => {
  return (
    <>
      <div>Article page:</div>
      <Article id={match.params.id} />
      <Link to="/articles">Back to list of articles</Link>
    </>

  );
}

export default ArticlePagComponent;
```
Notes:
- in props there is object "match", that has data from router (id) in property "params". So We destructurize props "({match})" and then take only match object, and from its params We get id "<Article id={match.params.id} />"

Good, now We can render article that will get this id in props:

```js
import React from 'react';

const Article = (props) => {
  return (
    <article className="article">
      <h1>Article id: {props.id}</h1>
    </article>
  );
}
export default Article;
```

Great! This way it will generate dymaically subpages, with different ids. Of course We have to get object article with all fields to show it with specific id...

That's all!



