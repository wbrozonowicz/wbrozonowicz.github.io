---
title: "Next.js - example of responsive Image"
excerpt: "How to create responsive Image in NEXT.JS"
last_modified_at: 2023-02-23T10:27:01-05:00
categories:
  - Next.js

tags: 
  - basics
---

<!-- short introduction -->
## Next.js - example of responsive image

First put image in folder "public" (the same level in project as folder src when use app router):

Next We will create page Team in "src/app/team" in file "page.tsx":

```js
import Image from "next/image";
import styles from "./team.module.scss";

export function generateMetadata() {
  return {
    title: "Team",
    description: "This is team page",
  };
}

export default function teamPage() {
  return (
    <div>
      <h1>teamPage</h1>
      <div className={styles["person"]}>
        <div className={styles["person-img"]}>
          {/* <img src="/team.jpg" alt="team" /> */}
          <Image
            src="/team.jpg"
            alt="team"
            sizes="(min-width:800px) 300px, 680px"
            width={0}
            height={0}
          />
        </div>
      </div>
      <div>
        <p>Lorem ipsum.... Lorem ipsum.... Lorem ipsum.... Lorem ipsum.... Lorem ipsum.... Lorem ipsum....
        Lorem ipsum....Lorem ipsum....Lorem ipsum....Lorem ipsum....Lorem ipsum....Lorem ipsum....Lorem ipsum....
        Lorem ipsum....Lorem ipsum....Lorem ipsum....Lorem ipsum....Lorem ipsum....Lorem ipsum....Lorem ipsum....
        Lorem ipsum....Lorem ipsum....Lorem ipsum....Lorem ipsum....Lorem ipsum....Lorem ipsum....Lorem ipsum....
        Lorem ipsum....Lorem ipsum....Lorem ipsum....Lorem ipsum....Lorem ipsum....Lorem ipsum....Lorem ipsum....
        Lorem ipsum....Lorem ipsum....Lorem ipsum....Lorem ipsum....Lorem ipsum....Lorem ipsum....Lorem ipsum....
        Lorem ipsum....Lorem ipsum....Lorem ipsum....Lorem ipsum....Lorem ipsum....Lorem ipsum....Lorem ipsum....
        </p>
      </div>
    </div>
  );
}
```

Note: instead of normal <img> We used component Image from Next.js

We defined that for screen 800px and bigger it will be 300px image and row, else image will be on full screen width.
So We defined style in "team.module.scss" in the same folder:

{% include code-header.html %}
```css
.person {
    display: flex;
    flex-flow: column;
    gap:20px;


    @media (min-width:800px){
        flex-flow:row;
    }

    &-img {
        width: calc(100%-5vw);
    }

    @media (min-width:800px){
        width: 300px;
        flex: 1 0 300px;
    }
}
```
We also add style in global css like:

{% include code-header.html %}
```css
img {
    width: 100%;
    height:auto;
}
```

... so every image in app will be now responsive.

Component Image will define srcset automatically, and set lazy loading.
If We don't want lazy loading (loading only if image is currently visible on page fragment),
then We only need to add "priority" parameter to Image component.

----


That's all!

*INFO: This post is my notes based on Udemy course ("Nextjs 14 od podstaw") by Jarosław Juszkiewicz
