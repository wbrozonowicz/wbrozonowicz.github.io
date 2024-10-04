---
title: "Next.js - fetch client and encodeURIComponent function"
excerpt: "Fetch data in nextjs, deal with slash in url"
last_modified_at: 2026-01-22T10:27:01-05:00
categories:
  - Next.js

tags: 
  - basics
---

<!-- short introduction -->
## Next.js - fetch client and encodeURIComponent function

Let's delegate fetch from api functionality to separate function:

- file: "src/common/clientApi/fetchClient.ts"

In file We will define and export function, that We will use in all other componnets to fetch data from API:

{% include code-header.html %}
```js
import { notFound } from "next/navigation";

type FetchClientOptions = {
  // in options client will get revalidate param (sec set for cache)
  revalidate: number;
};

export async function fetchClient<P = unknown>( // using Generic <P=unknown> We will be able to use client with all types of data
  url: string,
  options: FetchClientOptions = { revalidate: 10 } // default value for revalidate is 10
) {
  const { revalidate } = options; // destructure - take number value from options
  const responseData = await fetch(url, { next: { revalidate } });

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
```

OK, now We can use it this way (after import this function):

{% include code-header.html %}
```js
// get array of objects of type "Posts" (Post[]) from API
const posts = await fetchClient<Posts>('url_to_posts',{revalidate:5})

// get single object of type "Post" in separate function:
const fetchPost = async (postId:number) =>{`url_to_posts${postId}`
  return await fetchClient<Post>()
}
```

If We need to pass to url name with "/" inside We have to use function encodeURIComponent.
If not browser will use it as route not name:

 f.e:
{% include code-header.html %}
```js
type content ={
  id:string,
  title:string,
  body:string
}

let nameWithSlash = "good/bad"
const content = await fetchClient<content>(`url_${encodeURIComponent(nameWithSlash)}`)
```

That's all!

*INFO: This post is my notes based on Udemy course ("Nextjs 14 od podstaw") by Jarosław Juszkiewicz
