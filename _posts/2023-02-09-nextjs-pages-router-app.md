---
title: "Next.js - example of app using Pages Router"
excerpt: "How to create app that uses Pages Router in NEXT.JS"
last_modified_at: 2023-02-23T10:27:01-05:00
categories:
  - Next.js

tags: 
  - basics
---

<!-- short introduction -->
## Next.js - example of Pages router app

Pages Router is older approach then App Router - can be found in many older projects.
It is a little bit different then App Router.

1. Basic app structure:

Top level folder contains:
- Code is in folder "src"
- Images etc are in folder public
- node_modules
-".next" : folder for build
-".git"
- other files: gitignore, package.json etc


Folder src:
- folder "pages" for each page
- folder "styles: for css
- folder "common" for widely used components
- folder "types" for types that we want use accross app

Folder "common":
- file "config.ts" with constants that are used in app f.e.:
```js
export const POSTS_TOTAL = 30; // normally get this from API
```

- "clientApi" contains f.e. "fetchClient.ts" -> for fetching data

{% include code-header.html %}
```js
import { notFound } from "next/navigation";

// difference with this and App router: there is no cache and options for it in Pages Router!!
export async function fetchClient<P = unknown>( // using Generic <P=unknown> We will be able to use client with all types of data
  url: string) {
  const responseData = await fetch(url);
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
  method: "POST" | "PATCH",
  url: string,
  data: unknown
) {
  const resp = await fetch(url, {
    method,
    body: JSON.stringify(data),
    headers: { Accept: "application/json", "Content-Type": "application/json" },
  });
  if (!resp.ok) {
    console.log(resp);
    throw new Error(`Problem with update data, statis code: ${resp.status}`);
  }
}

export function generatePostTag(postId: number) {
  return `post${postId}`;
}
```


Folder "components":

- menu.tsx 

{% include code-header.html %}
```js
import Link from "next/link";
import style from "./menu.module.scss"

export function Menu() {
    return (
        <ul className={style.menu}>
            <li className={style["menu-item"]}>
                <Link href="/">Home</Link>
            </li>
            <li className={style["menu-item"]}>
                <Link href="/contact">Contact</Link>
            </li>
            <li className={style["menu-item"]}>
                <Link href="/contact/team">Team</Link>
            </li>
            <li className={style["menu-item"]}>
                <Link href="/contact/about-us">About us</Link>
            </li>
            <li className={style["menu-item"]}>
                <Link href="/posts">Posts</Link>
            </li>
        </ul>
    )
}
```

- "menu.module.scss" - style for menu:
{% include code-header.html %}
```css
.menu {
  display: flex;
  gap: 15px;
  list-style: none;
  background-color: rgb(35, 33, 33);
  padding: 15px 5vw;

  .menu-item a {
    color: white;
    font-size: 18px;
    text-decoration: none;
    font-weight: normal;
    &:hover {
      text-decoration: underline;
    }
  }
}
```

- folder "layouts" contains specific layouts for app f.e. mainLayout.tsx:
{% include code-header.html %}
```js
import { PropsWithChildren } from "react";
import { Menu } from "../menu";

type MainLayoutProps = {} & PropsWithChildren;

export const MainLayout = ({ children }: MainLayoutProps) => {
  return (
    <div className="main-container">
      <Menu></Menu>
      {children}
    </div>
  );
};

```



Folder "styles" contains "global.scss":

{% include code-header.html %}
```css
// @font-face {
//     font-family:  "Oswald"
//     src: url(./Oswald/Oswald-VariableFont_wght.ttf) format("truetype");
// }

* {
    box-sizing: border-box;
    padding:0;
    margin:0
}

html, body {
    font-family: Roboto, segoe ui,Helvetica, Arial, sans-serif, apple color emoji, segoe ui emoji, segoe ui symbol;
    color: rgb(53, 53, 53);
    font-size: 16px;
    font-weight: 300;
    line-height: 24px;
}

h1 {
    font-size: 2.5rem;
    margin-top: 20px;
    margin-bottom: 10px;
}

h2 {
    font-size: 2rem;
    margin-top: 20px;
    margin-bottom: 10px;
}

h3 {
    font-size: 1.5rem;
    margin-top: 20px;
    margin-bottom: 10px;
}

.main-container {
    margin: 30px 5vw;
}


.button {
    align-items:center;
    appearance: none;
    background-color: #fff;
    border-radius: 5px;
    border-style: none;
    box-shadow:
     rgba(0,0,0,0.2) 0 3px 5px -1px,
    rgba(0,0,0,0.2) 0 6px 10px 0,
    rgba(0,0,0,0.2) 0 1px 18px 0;
    box-sizing: border-box;
    color: #3c4043;
    cursor:pointer;
    display: inline-flex;
    fill:currentColor;
    font-family: "Google Sans", Roboto, Arial, sans-serif;
    font-size: 14px;
    font-weight: 500;
    height: 48px;
    justify-content: center;
    letter-spacing: 0.25px;
    line-height: normal;
    max-width: 100%;
    overflow: visible;
    padding: 2px 24px;
    position: relative;
    text-align:center;
    text-transform:none;
    transition: 
        box-shadow 280ms cubic-bezier(0.4,0,0.2,1),
        opacity 15ms linear 30ms,
        transform 270ms cubic-bezier(0,0,0.2,1) 0ms;
    user-select: none;
    -webkit-user-select: none;
    touch-action: manipulation;
    width:auto;
    will-change: transform,opacity;
    z-index:0;

    &:hover {
        background: #f6f9fe;
        color:#174ea6
    }

    &:disabled {
        background: rgba(0,0,0,0.3);
    }
}

    .form-line {
        display: flex;
        flex-direction: column;
        margin-bottom: 10px;

        label {
            font-weight: bold;
            font-size: 18px;
            line-height: 24px;
        }
        input,textarea {
            font-family: "Roboto";
            padding:14px 16px;
            font-size: 18px;
            line-height: 24px;
            max-width:100ch;
            letter-spacing:0.02em;
            border-radius: 3px;
        }
        &.error {
            input,textarea{
                border-color:red;

                &.focus,
                &.active {
                    border-color: red;
                    outline-color: red;
                }
            }
        }

        .error {
            display: block;
            color: red;
            font-weight:bold
        }
    }
```

And finally our "pages" folder:

1. files "_app.tsx" and "_document.tsx":

"_app.tsx" is file that adds something to every page in app. We can use it to add global style and something more.
In this file We have access to props of each page and to Components inside, so We can use it to dynamically interact with them.
 Ecample of file "_app.tsx":

{% include code-header.html %}
```js
import { AppProps } from "next/app";
import "../styles/global.scss";

// wrap every page in app with something
export default function MyApp({ Component, pageProps }: AppProps) {
  
  console.log("appp props=",pageProps)
  return (
    <div>
      This is app tsx // this will be on every page
      <Component {...pageProps} />
    </div>
  );
}
```
There is also file: "_document.tsx". Difference with it and "_app.tsx" is that this is static file and don't have access to any props of pages inside.

Example of "_document.tsx":

{% include code-header.html %}
```js
import { Html, Head, Main, NextScript } from "next/document";

export default function Document() {
  return (
    <Html lang="en">
      <Head>
        <body>
            This is document.tsx
          <Main />
          <NextScript />
        </body>
      </Head>
    </Html>
  );
}
```js

Text: "This is document.tsx" will be shown on the top of the page (higher level than text from "_app.tsx").

The _document.tsx file in Next.js is used to customize the HTML document structure
. It allows you to modify the <html>, <head>, and <body> tags that wrap your application
. This file is only executed on the server side, so it's ideal for adding elements that need to be present before the page loads, like meta tags, fonts, or scripts
n this example, the Document component updates the <html> tag to include a language attribute (lang="en") and ensures the <head>, <main>, and <next-script> tags are present

2. file "[...slug].tsx" for dynamic routing.
Whatever route We add after localhost:3000/ if there is no dedicated static route defined, this will be show:

{% include code-header.html %}
```js
import { MainLayout } from "@/common/components/layouts/mainLayout";

const InfoPage = () => {
  return (
<MainLayout>
        <h1> Info page [...slug]</h1>
    </MainLayout>
  );
};

export default InfoPage;
```

3. file "index.tsx" that will be our home page:

{% include code-header.html %}
```js
import { MainLayout } from "@/common/components/layouts/mainLayout";
import Link from "next/link";

const HomePage = () => {
  return (
    <MainLayout>
      <h1> Home page</h1>
      <Link href="/posts">Posts</Link>
    </MainLayout>
  );
};

export default HomePage;
```

Now: general rule with Page Router is:
- If we create folder "somepath" and put inside file "index.tsx" then
 it will create static route for url: localhost:3000/somepath

 But there is also second method:
 - If we create in some folder f.e. in main folder "pages" file "contact-out.tsx" then this will be
 also our route (localhost:3000/contact-out)

{% include code-header.html %}
```js
import { MainLayout } from "@/common/components/layouts/mainLayout";

const ContactPage = () => {
  return (
    <MainLayout>
      <h1>Contact-outside folder</h1>
    </MainLayout>
  );
};

export default ContactPage;
```

Ok, Let's dive deeper: in folder "pages" folder "posts" and inside it:
- file "index.tsx" -> home page for posts
- folder "[postId]": this will create dynamic route for post with specific id 

So file: "src/pages/posts/index.tsx":

{% include code-header.html %}
```js
import { fetchClient } from "@/common/clientApi/fetchClient";
import { MainLayout } from "@/common/components/layouts/mainLayout";
import { Posts } from "@/types/Posts";
import { InferGetServerSidePropsType } from "next";
import Head from "next/head";
import Link from "next/link";
import { useEffect } from "react";

//function runs only on server side, You can use it to get data from db etc...
export const getServerSideProps = async () => {
  const posts = await fetchClient<Posts>("http://localhost:3004/posts");
  return {
    props: {
      posts,
    },
  };
};

const PostsPage = ({
  posts
}: InferGetServerSidePropsType<typeof getServerSideProps>) => {

useEffect(()=>{
  console.log("You can run use effect in components in Pages Router")
})

  return (
    <MainLayout>
      <Head>
        <title>Post Page</title>
      </Head>
      <h1> Posts page </h1>
      {posts.map(post => (
        <div key={post.id}>
          <h2>{post.title}</h2>
          <Link href={`/posts/${post.id}`}>Read more</Link>
        </div>
      ))}
    </MainLayout>
  );
};

export default PostsPage;
```

Note: components in Pages Router are hybrid: partly server side / partly client side.
In Next.js, there are three main server-side functions you can use in the Pages Router to fetch data and handle requests:

- getStaticProps: This function is used for Static Site Generation (SSG). It fetches data at build time and pre-renders the page with this data. It's ideal for pages that don't change frequently

- getServerSideProps: This function is used for Server-Side Rendering (SSR). It fetches data on every request, making it suitable for pages with frequently updated content

- getStaticPaths: This function is used alongside getStaticProps for dynamic routes. It generates paths for the dynamic routes at build time, allowing you to pre-render pages for different route parameters

But besides this functions, component are client side, so We can Use inside hooks like "useEffect" in our example.
!! THIS WAS NOT POSSIBLE IN APP ROUTER (there We have to define in each component is it "use server" or "use client").


The next element in our simple example: in folder "src/pages/vlogs" file "[[...slug]].tsx":

{% include code-header.html %}
```js
import { MainLayout } from "@/common/components/layouts/mainLayout";

const VlogsPage = () => {
  return (
    <MainLayout>
      <h1> Vlogs page [[...slug]]</h1>
    </MainLayout>
  );
};

export default VlogsPage;
```

This is example for route, that except f.e. route:
- "localhost:3000/vlogs"
and
- "localhost3000/vlogs/"
and
- "localhost:3000/vlogs/somepaths" etc

So once again:

If you create a file named [[...slug]].tsx in a Next.js app, it will function as an optional catch-all route. This means the file will handle both the base path and any nested paths.

Example
File: pages/shop/[[...slug]].tsx
Matched Routes:
/shop → slug: []
/shop/clothes → slug: ['clothes']
/shop/clothes/tops → slug: ['clothes', 'tops']
This allows you to manage multiple levels of nested routes with a single component, making your routing more flexible and efficient.

Creating a file named [...slug].tsx in a Next.js app sets up a catch-all route. This means the file will handle any path that starts with the specified prefix and captures all subsequent path segments into an array.

Example
File: pages/shop/[...slug].tsx
Matched Routes:
/shop/clothes → slug: ['clothes']
/shop/clothes/tops → slug: ['clothes', 'tops']
This is useful for creating dynamic routes that can handle multiple levels of nested paths.


OK, now example with other 2 server side functions:

1. "getStaticProps"

- Create folder "info"in src/pages
- inside create file "index.tsx":

{% include code-header.html %}
```js
import { fetchClient } from "@/common/clientApi/fetchClient";
import { MainLayout } from "@/common/components/layouts/mainLayout";
import { Contents } from "@/types/Contents";
import { InferGetStaticPropsType, NextPage } from "next";
import Link from "next/link";

// this functions runs only once on server side, during build
// use this when data are changed very rarely
// optionally we can define revalidate to refresh cache and regenerate
export const getStaticProps = async () => {
  const pages = await fetchClient<Contents>("http://localhost:3004/contents");
  return {
    props: {
      pages,
      randomNumber: Math.random().toString(), // this will be calculated only once during build
    },
    revalidate: 15 //15 sec refresh cache
  };
};

const InfoPage: NextPage<InferGetStaticPropsType<typeof getStaticProps>> = ({
  pages,
  randomNumber,
}) => {
  return (
    <MainLayout>
      <h1>Info pages</h1>
      <h2>randomNumber: {randomNumber}</h2>
      <div>
        {pages.map(page=>(
            <div key={page.id}>
                {page.title}
                <Link href={`/info/${page.id}`}>{page.title}</Link>
            </div>
        ))}
      </div>
    </MainLayout>
  );
};

export default InfoPage;

```


2. "getStaticPaths"
If We use dynamic routing, generated by data from API, then when new data are fetched, new paths will be not available.
They need to be generate.
We can use third function called "getStaticPaths"

- create folder [slug] inside folder "info"
- create "index.tsx" inside [slug]

{% include code-header.html %}
```js
import { fetchClient } from "@/common/clientApi/fetchClient";
import { MainLayout } from "@/common/components/layouts/mainLayout";
import { Content, Contents } from "@/types/Contents";
import { InferGetStaticPropsType, NextPage } from "next";

type StaticProps = {
  params: {
    slug: string;
  };
};

// this will get all paths from API in order to generate static paths during build
// if we use it with optional parameter 'fallback' we can regenerate new page if it will be added to api
// f.e. in different place we revalidate contents and generate new link to page with it
// if static paths from build don't have this new path, there will be error
// but fallback "blocking" will generate this new path and show to user (and save to cache)
export const getStaticPaths = async () => {
  const pages = await fetchClient<Contents>(`http://localhost:3004/contents`);
  const paths = pages.map((page) => {
    return {
      params: {
        slug: page.id,
      },
    };
  });
  return {
    paths,
    fallback: 'blocking', // false= do not generate new page if not exist, true: generate but no refresh, 'blocking': generate and wait untill is ready
  };
};

// this will return specific prop (page data with dynamic id = slug )
export const getStaticProps = async ({ params }: StaticProps) => {
  const page = await fetchClient<Content>(
    `http://localhost:3004/contents/${params.slug}`
  );
  return {
    props: {
      page,
    },
  };
};

const InfoPage: NextPage<InferGetStaticPropsType<typeof getStaticProps>> = ({
  page,
}) => {
  return (
    <MainLayout>
      <h1>{page.title}</h1>
      <p>{page.body}</p>
    </MainLayout>
  );
};

export default InfoPage;
```

Some explanation:

- getStaticPaths:

Fetches all content paths from the API.
Generates paths for each content item based on the id.
Returns these paths with fallback: 'blocking', allowing new pages to be generated if they don't exist yet.

- getStaticProps:
Fetches specific content based on the slug parameter.
Returns the content as props for the page.

- InfoPage Component:
Uses the fetched content to render a page with a title and body within the MainLayout.


NOW: API in Pages Router:

- create folder "api" in src/pages
- inside create file "hello.ts"

File as below:

{% include code-header.html %}

```js
import { NextApiRequest, NextApiResponse } from "next";

type ResponseData = {
  message: string;
};

export default function handler(
  req: NextApiRequest,
  res: NextApiResponse<ResponseData>
) {
  if (req.method === "POTS") {
    res.status(200).json({ message: "Hello - it is POST method" });
  } else if (req.method === "GET") {
    res.status(200).json({ message: "Hello - it is GET method" });
  }
}
```

Now if We send GET request to "http://localhost:3000/api/hello" We will get in response json object:
{
    "message": "Hello - it is GET method"
}


----


That's all!

*INFO: This post is my notes based on Udemy course ("Nextjs 14 od podstaw") by Jarosław Juszkiewicz
