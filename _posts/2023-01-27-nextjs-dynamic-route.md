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

That's all!

*INFO: This post is my notes based on Udemy course ("Nextjs 14 od podstaw") by Jarosław Juszkiewicz
