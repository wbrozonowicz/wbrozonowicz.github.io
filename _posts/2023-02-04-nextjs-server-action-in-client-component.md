---
title: "Next.js - server action example used in client component"
excerpt: "Simple example of server action in client component, cache revalidate by tag"
last_modified_at: 2023-02-22T10:27:01-05:00
categories:
  - Next.js

tags: 
  - basics
---

<!-- short introduction -->
## Next.js - server action example used in client component"

Server actions are performed on server, outside user browser. But We can run them from client component. Example:

1. Let's create user client component "LikePost.tsx"

{% include code-header.html %}
```js
"use client";

import { FC, useCallback } from "react";
import { likePost } from "./actions";

type LikePostProps = { postId: number };

export const LikePost: FC<LikePostProps> = ({ postId }) => {
  const onLike = useCallback(async () => {
    await likePost(postId);
  }, [postId]);

  return (
    <div>
      <button className="button" onClick={onLike}>
        <img src="/like.png" alt="like+1" height={16} width={16} />
      </button>
    </div>
  );
};
```

2. Our action is defined in file "actions.ts" in the same folder:

{% include code-header.html %}
```js
"use server";

import { fetchClient, generatePostTag, updateClient } from "@/common/clientApi/fetchClient";
import { Post } from "@/types/Posts";
import { revalidatePath, revalidateTag } from "next/cache";

export const likePost = async (postId: number) => {

  const post = await fetchClient<Post>(`http://localhost:3004/posts/${postId}`); // get Post object from API 

  await updateClient("PATCH", `http://localhost:3004/posts/${postId}`, { // send to API path for this object with reactions +1
    reactions: post.reactions + 1,
  });

  revalidateTag(generatePostTag(postId)) // update cached values with specific tag [all values with this tags even on different paths] - so page refresh is automatically done on function likePost call

  // revalidatePath(`/posts/${postId}`) // update cached values on whole path [all values no mettr what tags are but on specific path]- so page refresh is automatically done on function likePost call
};
```

3. In order to revalidate strategy work, fetchClient (function to fetch data) and updateClient (function to post or patch data) need to be updated like this:

File: fetchClient.ts

{% include code-header.html %}
```js
import { notFound } from "next/navigation";

type FetchClientOptions = {
  // in options client will get revalidate param (sec set for cache)
  revalidate?: number;
  // or use tags to revalidate cached request
  tags?: string[];
};

export async function fetchClient<P = unknown>( // using Generic <P=unknown> We will be able to use client with all types of data
  url: string,
  options: FetchClientOptions = { revalidate: 10 } //default cache revalidation
) {

  const { revalidate, tags } = options; // destructure - take revalidate number value from options or tags
  const responseData = await fetch(url, { next: { revalidate, tags } }); // use in options revalidate or tags - both props are optional
  if (!responseData.ok && responseData.status === 404) {
    //not found error handling
    throw notFound();
  }
  if (!responseData.ok) {
    throw new Error("Something went wrong when fetching fromM API");
  }
  const data: P = await responseData.json();
  return data;
}

export async function updateClient(
  method: "POST" | "PATCH" | "PUT", // define type of request: POST-> create, PATCH-> update object by sending only some of his properties, PUT-> pdate object by sending all of his properties
  url: string,
  data: unknown // object do send
) {
  const resp = await fetch(url, {
    method,
    body: JSON.stringify(data),
    headers: { Accept: "application/json", "Content-Type": "application/json" }, // define headers - format send ("json"), format expected ("json")
  });
  if (!resp.ok) {
    console.log(resp);
    throw new Error(`Problem with update data, statis code: ${resp.status}`);
  }
}

// simple function to generate tag string
export function generatePostTag(postId:number) {
  return `post${postId}`
}
```

4. Now We can use all of this on our Page:

{% include code-header.html %}
```js
import { Post } from "@/types/Posts";
import style from "./post.module.scss";
import { LikePost } from "../LikePost"; //import our Client Component with server action
import { fetchClient, generatePostTag } from "@/common/clientApi/fetchClient";

type PostPageProps = {
  params: {
    postId: string;
  };
};

// function to fetch post, but with usage of tags
const fetchPost = async (postId: number) => {
  return await fetchClient<Post>(`http://localhost:3004/posts/${+postId}`, {
    tags: [generatePostTag(postId)],
  });
};

export const generateMetadata = async ({ params }: PostPageProps) => {
  const post = await fetchPost(+params.postId);

  return {
    title: post.title,
    description: `read about ${post.title}`,
  };
};

const PostPage = async ({ params }: PostPageProps) => {
  const post = await fetchPost(+params.postId);
  return (
    <div>
      <h1>Post {params.postId}</h1>
      <p>{post.body}</p>
      {post.tags && post.tags.length > 0 && (
        <div className={style.tags}>
          {post.tags.map((tag) => (
            <em key={tag}>{tag}</em>
          ))}
        </div>
      )}
      <div className={style.likes}>Likes: {post.reactions}</div>
      <LikePost postId={post.id} /> 
    </div>
  );
};

export default PostPage;
```

That's all!

*INFO: This post is my notes based on Udemy course ("Nextjs 14 od podstaw") by Jarosław Juszkiewicz
