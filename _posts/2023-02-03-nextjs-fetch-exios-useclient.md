---
title: "Next.js - fetch data by axios and use client component"
excerpt: "Fetch data in nextjs with axios library in client component"
last_modified_at: 2023-02-22T10:27:01-05:00
categories:
  - Next.js

tags: 
  - basics
---

<!-- short introduction -->
## Next.js - fetch data by axios and use client component"

Client components are traditional way React works (before Next.js came in): javascript is send to browser and on trhe client side (in browser) html is generated.
On the other hand, server side component (default in Next.js) works this way, that html is generated on the server and result html is send to client.

Advantages of server side components: better for SEO, good for api requests
Advantages of client side components: better for fast interactions for user and state management is easier

In Next.js We can use both way and combine them.
To make component "client component" it is only required to add at the begining of the file one line:

{% include code-header.html %}
```js
"use client";
```

If We include other components inside it, they will also be client components!

In client components it is good to use for fetching data axios library and below example how to do this:
{% include code-header.html %}
```js
""use client";
import React, { useCallback, useEffect, useState } from "react";
import axios from "axios";
import { Posts } from "@/types/Posts";
import Link from "next/link";
import style from "./lastNews.module.scss";
import { POSTS_TOTAL } from "../config";

export const LastNews = () => {
  const [posts, setPosts] = useState<Posts | undefined>(undefined);
  const [page, setPage] = useState(1);
  const POST_LIMIT = 3;

// useCallback hook to optimize function usage - function definition is gnerated only if needed [if page is changed], not with every render
  const fetchNews = useCallback(async () => {
    const resp = await axios.get<Posts>("http://localhost:3004/posts", {
      params: { _limit: POST_LIMIT, _page: page },
    });
    setPosts(resp.data);
  }, [page]);

  useEffect(() => {
    fetchNews();
  }, [fetchNews, page]);

  const onNextNews = useCallback(() => { 
    setPage((prevPage) => {
      if (prevPage + 1 < Math.ceil(POSTS_TOTAL / POST_LIMIT)) {
        return prevPage + 1;
      }
      return 1;
    });
  }, []);

  const onPrevNews = useCallback(() => {
    setPage((prevPage) => {
      if (prevPage - 1 > 0) {
        return prevPage - 1;
      }
      return Math.ceil(POSTS_TOTAL / POST_LIMIT);
    });
  }, []);

  return (
    posts && (
      <div className={style["last-news"]}>
          <button className="button" onClick={onPrevNews}>Prev News</button>
        {posts.map((post) => (
          <div className={style.item} key={post.id}>
            {post.id}-{post.title}
            <div className={style["item-link"]}>
              <Link href={`/posts/${post.id}`}>Read more</Link>
            </div>
          </div>
        ))}use client";
        <button className="button" onClick={onNextNews}>
          Next news
        </button>
      </div>
    )
  );
};
```


and scss file:

{% include code-header.html %}
```css
.last-news {
  display: flex;
  flex-flow: row wrap;
  justify-content: space-around;
  align-items: center;

  .item {
    flex-basis: 250px;
    min-height: 120px;
    padding: 10px 15px;
    border: 2px solid gray;
    border-radius: 5px;
    font-weight: bold;
    font-size: 0.9 rem;

    display: flex;
    flex-direction: column;
    justify-content: space-between;

    &-link {
      width: 100%;
      text-align: right;
    }
  }
}
```

 - "&-link" - The &-link inside .item will compile to .item-link. This means that the & is replaced by the parent selector .item, resulting in .item-link




That's all!

*INFO: This post is my notes based on Udemy course ("Nextjs 14 od podstaw") by Jarosław Juszkiewicz
