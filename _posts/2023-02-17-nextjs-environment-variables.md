---
title: "Next.js - example of environment variables"
excerpt: "How to use .env in NEXT.JS"
last_modified_at: 2023-02-23T10:27:01-05:00
categories:
  - Next.js

tags: 
  - basics
---

<!-- short introduction -->
## Next.js - example of .env

We have 3 levels ov environment variable files:

1. file: ".env"
2. file: ".env.production"
3. file: ".env.local"

All these files should be at top level of directory of our app so on the same level as "src" folder.
First file".env" - it is general file that is read before other two files. It contains default values. It is generally accessible only by server (with exception).

Second file ".env.production" - it is file with values needed in production environment.It is read in the second step, and all values with the same key as in ".env" will be overwritten with values from ".env.production" - of course if We run app in production environment. It also generally could be read only by server side.

Exception: If We define value with prefix "NEXT_PUBLIC_" than We make this key available publicly. f.e.

```js
API_URL="http://localhost:3004"
NEXT_PUBLIC_API_URL="http://localhost:3004"
```
Warning:
DO NOT USE PREFIX "NEXT_PUBLIC" TO ANY SECRET!


When We will need public access to these values? In "use client" components, If We need access to API inside them.

Third file called ".env.local" - it is file that is read in the third step and all values that were in previous files, with the same keys, in local environment will be overwritten with values from this file.
Note: by default this file is defined in gitignore, so it shouldn't be store in remote repositories.

OK, now how to use them in app?

We can store secret keys, passwords etc. But also it is good place to define url to API inside it.

Example:
- We can create file "config.ts" in "src/common" folder
- Inside We can define:
```js
export const API_URL = process.env.API_URL || "http://localhost:3004";
export const PUBLIC_API_URL = process.env.NEXT_PUBLIC_API_URL || "http://localhost:3004";
```
Next We can use it in our fetchClient function, that We use to make all request:

```js
import { API_URL } from "../config"
...
// use in function
const resp = await fetch(`${API_URL}${url}`,{next: {revalidate,tags}});
```

Thanks to this, When We use this function, instead of full url We can just use:

```js
const post = await fetchClient<Post>(`/posts/${postId}`);
```

Note: in client component We have to use public env so:

```js
import { PUBLIC_API_URL } from "../config";
...
// use in function
const resp = await axios.get<Posts>(PUBLIC_API_URL + "/posts", {
  params:{_limit: POST_LIMIT, _page: page}
})
```

That's all!

*INFO: This post is my notes based on Udemy course ("Nextjs 14 od podstaw") by Jarosław Juszkiewicz
