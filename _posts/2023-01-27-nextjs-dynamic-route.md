---
title: "Next.js - dynamic route"
excerpt: "Fetch data in metadata & page, use dynamic route"
last_modified_at: 2026-01-22T10:27:01-05:00
categories:
  - Next.js

tags: 
  - basics
---

<!-- short introduction -->
## Next.js - dynamic route

First We have to make folder that will store our dynamic page, f.e. if We want url: "/posts/:id" then:

- create folder "src/app/posts/[postId]"
- in folder [postId] create file page.tsx (and "post.module.scss" for styling)
- then in create page We will get postId from params:

{% include code-header.html %}
```js
import { Post } from "@/types/Posts";
import { NextPage } from "next";

import style from "./post.module.scss"
import { notFound } from "next/navigation";

type PostPageProps = {
    params:{
        postId:string; //the same name as folder name = postId
    }
}

// fetch post here so We can use it also in metadata
const fetchPost = async (postId:number) => {
    const resp = await fetch(`http://localhost:3004/posts/${+postId}`)
    // show default Next.js page for not found item [404]
    if(!resp.ok && resp.status===404){
        throw notFound() 
    }
    if(!resp.ok){
        throw new Error("problem with fetching post")
    }
    const post:Post = await resp.json();
    return post
}


// create metadata with data fetched from API
export const generateMetadata = async ({params}:PostPageProps)=>{
    const post = await fetchPost(+params.postId) // "+" makes number
    return {
        title:post.title,
        description:`read about ${post.title}`
    }
}

// 1st method of props declaration
const PostPage = async ({params}:PostPageProps) => {
  // get also data in page (fetch has cache so it is not double execution)
    const post = await fetchPost(+params.postId) // "+" makes number
    return (
        <div>
          <h1>Posts {params.postId}</h1>
          <p>{post.body}</p>
          {post.tags && post.tags.length>0 && (
              <div className={style.tag}>
                  {post.tags.map((tag)=> (<em key={tag}>{tag}</em>))}
              </div>
          )}
        </div>
      );
};

export default PostPage;
```

We can declare props also this way:

{% include code-header.html %}
```js
// 2nd method of props declaration
const PostPage:NextPage<PostPageProps> = async ({params}) => {
    const post = await fetchPost(+params.postId)
    return (
      <div>
        <h1>Posts {params.postId}</h1>
        <p>{post.body}</p>
        {post.tags && post.tags.length>0 && (
            <div className={style.tag}>
                {post.tags.map((tag)=> (<em key={tag}>{tag}</em>))}
            </div>
        )}
      </div>
    );
//   };
  
  export default PostPage;
```

OK. Some styling:
{% include code-header.html %}
```css
.tag {
    margin-top: 20px;
em {
    margin-right: 10px;
    background-color: bisque;
    border-radius: 3px;
    padding: 5px;
}
}
```

Now:
We can define path that will CATCH ALL paths nad give us them in params.

How:if We make folder with brackets in name f.e. "[...path]",
then We will catch all paths from url address and app-router will redirect to page.tsx, that is defined in this folder.
For example:

folder "app/[...path]"
- url: "localhost:3000/something" will redirect to page in folder [...path] when there will be no other folder named "something".

folder "/app/info/[...path]"
- url: "localhost:3000/info/whatever" will redirect to page in folder /info/[...path] when there will be no other folder named "whatever".
- url: "localhost:3000/info" will not work this way - only when there will be slash on the end (localhost:3000/info/") it will redirect to "/info/[...path]", without slash it will still redirect to "app/[...path]"

folder "/app/info/[[...path]]" - app-router will redirect also with url: "localhost:3000/info" to page in folder: "/app/info/[[...path]]" (and of course all other urls will do the same f.e. "localhost:3000/info/3/4/wtf/667" etc)

How to read params from such urls?

Let's see example:
1. "app/[...path].page.tsx"


{% include code-header.html %}
```js
import { NextPage } from "next";

type InfoPageProps = {
  params: {
    path: string[];
  };
};
const InfoPage: NextPage<InfoPageProps> = ({ params }) => {
  return (
    <div>
      <h1>Page</h1>
      params: {params.path.join("/")}
    </div>
  );
};

export default InfoPage;
```

Ok, with "[[...path]]" there is small difference: We need to check if params.path exist, so file "/app/info/[[...path]]" will be as below:

{% include code-header.html %}
```js
import { NextPage } from "next";

type InfoPageProps = {
  params: {
    path: string[];
  };
};
const InfoPage: NextPage<InfoPageProps> = ({ params }) => {
  return (
    <div>
      <h1>Info / Page</h1>
      params: {params.path && params.path.join("/")}
    </div>
  );
};

export default InfoPage;
```


That's all!

*INFO: This post is my notes based on Udemy course ("Nextjs 14 od podstaw") by Jarosław Juszkiewicz
