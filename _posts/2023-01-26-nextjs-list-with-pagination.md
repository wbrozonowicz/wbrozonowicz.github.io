---
title: "Next.js - list & pagination"
excerpt: "Fetch data, list it and use pagination"
last_modified_at: 2026-01-22T10:27:01-05:00
categories:
  - Next.js

tags: 
  - basics
---

<!-- short introduction -->
## Next.js - list & pagination

Let's create component "Pagination.tsx" in "src/common/components/" 

{% include code-header.html %}
```js
import Link from "next/link";
import style from "./pagination.module.scss"

type PaginationProps = {
  page: number;
  total: number;
  perPage: number;
};

export const Pagination = ({ page, total, perPage }: PaginationProps) => {
  const isNextPage = (page) * perPage <total;
  return (
    <div className={style.pagination}>
      {page > 1 && <Link href={`/posts?page=${page - 1}`}>Prev</Link>}
      <div>page: {page}</div>
      {isNextPage && <Link href={`/posts?page=${page + 1}`}>Next</Link>}
    </div>
  );
};
```

...with style: ("pagination.module.scss")

{% include code-header.html %}
```css
.pagination {
    display: flex;
    justify-content: space-between; 
}
```

OK. Now Use it on page where We show list:

"src/app/posts/page.tsx"

{% include code-header.html %}
```js
import { Posts } from "@/types/Posts";
import style from "./posts.module.scss";
import { commonMetadata } from "@/common/shared-metadata";
import { Pagination } from "@/common/components/Pagination";
import { SearchParams } from "@/types/NextTypes";

export const metadata = {
  title: `Posts from API ${commonMetadata.title}`,
  description: "Post list",
};

type PostsPageProps = {} & SearchParams; // join propoerties of first object (this case empty obj) with SearchParams type

const POSTS_TOTAL = 30; // normally get this from API
const POST_PER_PAGE = 10;

export default async function PostPage({ searchParams }: PostsPageProps) {
  let page = 1;
  if (searchParams?.page) { // check if there are any params in url
    page = Number(searchParams?.page) || 1; // if no params in url -> just take 1
  }

// fetch data from API
  const res = await fetch(
    `http://localhost:3004/posts?_limit=${POST_PER_PAGE}&_page=${page}`, //jsonServer - mock of real api with data offset
    { next: { revalidate: 5 } } // refresh cached values every 5 seconds, without this option cache will be till restart next.js server [by default]!!
  );

  if (!res.ok) { // check if there is error in response (no "ok" property)
    throw new Error("problem with fetching posts");
  }

  const posts: Posts =  await res.json(); // cast response to Posts (Post[]) 

  return (
    <div>
      <h1>Posts</h1>
      {posts.map((post) => (
        <div className={style.item} key={post.id}>
          {post.id};{post.title}
        </div>
      ))}

      <Pagination page={page} total={POSTS_TOTAL} perPage={POST_PER_PAGE} />
    </div>
  );
}

```

... and style ("posts.module.scss"):

{% include code-header.html %}
```css
.item {
    padding: 0 15px 24px 15px;
    border-bottom: 1px solid gray;
    margin:24px;
}
```
In example We use type: SearchParams
It is declared for all components in our app in "src/types/NextTypes.ts":

{% include code-header.html %}
```js
export type SearchParams = {
  searchParams?: { [key: string]: string | string[] | undefined };
};
```
In TypeScript, the type definition searchParams?: { [key: string]: string | string[] | undefined }; means that searchParams is an optional object where each key is a string, and the value associated with each key can be either a string, an array of strings, or undefined.

Here’s a breakdown:

- searchParams?: The ? indicates that searchParams is optional. It may or may not be present.
- { [key: string]: string | string[] | undefined }: This defines an object type where:
[key: string]: The keys of the object are strings.
- string | string[] | undefined: The values can be a string, an array of strings, or undefined.

This type is useful for scenarios where you might have query parameters in a URL, and each parameter can have multiple values or might not be present at all.

Another type that We use is "Posts" from "src/types/Posts.ts":

{% include code-header.html %}
```js
export type Post = {
    id:number,
    title:string,
    body:string,
    userId: number,
    tags: string[],
    reactions:number
}

export type Posts = Post[];
```

Function is async -> it's using "await" ->  otherwise fetch will be Promise.

Remember about default cache and specify in featch, after comma options in form of object:
{ next: { revalidate: 5 } } 

That's all!