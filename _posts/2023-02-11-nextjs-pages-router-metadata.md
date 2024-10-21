---
title: "Next.js - example of use of metadata in Pages Router"
excerpt: "How to create title &  meta data etc. in app that uses Pages Router in NEXT.JS"
last_modified_at: 2023-02-23T10:27:01-05:00
categories:
  - Next.js

tags: 
  - basics
---

<!-- short introduction -->
## Next.js - example of Pages router app - metadata

If We have components wrap up with MainLayout, we can set up metadata in this file and We will have default meta for all pages:

- file "config.ts" with constants that are used in app f.e.:

```js
import { PropsWithChildren } from "react";
import { Menu } from "../menu";
import Head from "next/head";

type MainLayoutProps = {} & PropsWithChildren;

export const MainLayout = ({ children }: MainLayoutProps) => {
  return (
    <div className="main-container">
      <Head>
        <title>Default APP  title</title>
        <meta name="description" content="default layout description"/>
      </Head>
      <Menu></Menu>
      {children}
    </div>
  );
};
```

Note: proper import is important: should be "import Head from "next/head";"!!


If we want to overwrite this only in specific page, We can make it like this:

F.e. in PostPage that shows details of specific post:

{% include code-header.html %}
```js
const PostPage = ({post}:InferGetServerSidePropsType<typeof getServerSideProps>) => {
  return (
    <MainLayout>
      <Head>
        <title>
          {post.title}
        </title>
      </Head>
      <h1>{post.title}</h1>
      <p>
        {post.body}
      </p>
    </MainLayout>
  );
};

export default PostPage;
```
This will overwrite only title, description will stay default.


----


That's all!

*INFO: This post is my notes based on Udemy course ("Nextjs 14 od podstaw") by Jarosław Juszkiewicz
