---
title: "Next.js - example of form with protection against CSRF attacks"
excerpt: "How to create form that is protected ahainst SCRF attack in NEXT.JS"
last_modified_at: 2023-02-22T10:27:01-05:00
categories:
  - Next.js

tags: 
  - basics
---

<!-- short introduction -->
## Next.js - example of form with protection against CSRF attacks

First create Form.component. This will be one component used for both: create new record and update existing record.


1. Let's create user client component "Form.tsx" in src/app/posts/new/

{% include code-header.html %}
```js
"use client";

import { FC, useCallback, useState, useEffect } from "react";
import { PostData, savePost } from "../actions";
import { useFormState, useFormStatus } from "react-dom";
import { FormState, Post } from "@/types/Posts";
import { ErrorForm } from "@/common/components/form/ErrorForm";
import axios from "axios";

export const FormPost: FC<{ post?: Post }> = ({ post }) => {

  //token for protecting against csrf attack
  const [csrfToken, setCsrfToken] = useState("");

  // chceck if csrf token is valid and send data
  const saveMyPost = async (
    prevState: FormState,
    formData: FormData
  ): Promise<FormState> => {
    try {
      // first verify token before submit data
      const response = await axios.post(
        "http://localhost:3000/api/token_verification",
        { data: "chceck my token please" },
        {
          headers: {
            "X-XSRF-TOKEN": `${formData.get("csrfToken")}`, // ustawienie nagłówka z tokenem XSRF
          },
          withCredentials: true,
        }
      );
      if (response.status === 200) {
        // check if api confirmed proper token
        if (response.data.message === "TOKEN_VALID") {
          return await savePost(prevState, formData);
        } else {
          return {
            ...prevState,
            message: "API didn't send proper confirmation",
          };
        }
      } else {
        // something went wrong
        return { ...prevState, message: "CSRF confirmation failed" };
      }
    } catch (error) {
      return {
        ...prevState,
        message: `Error during CSRF verification: ${error}`,
      };
    }
  };

  // use formAction to control pending state of form etc.
  const [state, formAction] = useFormState<FormState, FormData>(saveMyPost, {
    errors: {},
  });

  // when form is created first time, fetch csrf token from API
  useEffect(() => {
    fetch("../api/csrf", {
      headers: {
        Accept: "application/json",
      },
    })
      .then((res) => {
        return res.json();
      })
      .then((data) => {
        setCsrfToken(data.csrfToken);
      })
      .catch((error) => console.error("Error fetching CSRF token:", error));
  }, []);

  return (
    <form action={formAction}>
      <FormContent post={post} state={state} csrfToken={csrfToken} />
    </form>
  );
};


// subcomponent with form fields
const FormContent: FC<{ state: FormState; post?: Post; csrfToken: string }> = ({
  state,
  post,
  csrfToken,
}) => {
  const { pending } = useFormStatus(); // this hook useStatus return current status of form, so We know if it is now sending and can disable button

  // this set up values in fields, type will be PostData object that has all needed properties that are in form
  const [values, setValues] = useState<PostData>({
    title: post?.title || "",
    body: post?.body || "",
    tags: post?.tags.join(",") || "",  //tags are string[] -> here change it to string
  });

  // below function that set value to object that will be send
  const setValue = useCallback(
    (e: React.ChangeEvent<HTMLInputElement | HTMLTextAreaElement>) => {
      setValues((prev) => {
        return { ...prev, ...{ [e.target.name]: e.target.value } };
      });
    },
    [setValues]
  );


  return (
    <>
      <div className="form-line">
        {/* below hidden input with csrfToken generated by api */}
        <input readOnly hidden name="csrfToken" value={csrfToken} />
        <label>Title:</label>
        <input
          onChange={setValue}
          name="title"
          type="text"
          value={values.title}
        />
        <ErrorForm errors={state.errors.title} />
      </div>
      <div className="form-line">
        <label>Body:</label>
        <textarea
          onChange={setValue}
          name="body"
          value={values.body}
        ></textarea>
        <ErrorForm errors={state.errors.body} />
      </div>
      <div className="form-line">
        <label>Tags</label>

        <small>Separate by , (comma)</small>
        <input onChange={setValue} name="tags" value={values.tags} />
        <ErrorForm errors={state.errors.tags} />
      </div>
      {/* below hidden input with post id, thanks to this We can next check if send data has id
       (editing existing post) or not have (new post) */}
      <input type="hidden" name="id" value={post?.id} readOnly />
      {/* if form state is pending then adjust button text and disability */}
      <button disabled={pending} className="button">
        {pending ? "Saving" : "Save"}
      </button>
    </>
  );
};
```

2. Our API that will generate &  send token is simple, file: src/app/api/scrf/route.ts:

{% include code-header.html %}
```js
import { NextResponse } from "next/server";
import csrf from "csrf";

// create new instance of csrf object to generate csrf tokens
const tokens = new csrf();

// get secret from .env or generate one
const secret = process.env.CSRF_SECRET || tokens.secretSync();

export async function GET() {
  // create token with secret
  const token = tokens.create(secret);
      
  const response = NextResponse.json({ csrfToken: token });
  response.cookies.set('XSRF-TOKEN', token, { httpOnly: true, secure: true, sameSite: 'strict' });
  return response;
}
```

3. Ok, now: when data are sent to API, first app checks if token is OK. 
It is file: src/app/api/token_verification/route.ts:

{% include code-header.html %}
```js
import { NextRequest, NextResponse } from "next/server";
import csrf from "csrf";

const tokens = new csrf();
const secret = process.env.CSRF_SECRET || tokens.secretSync();

export async function POST(request: NextRequest) {
  
  // token form cookie
  const cToken = request.cookies.get("XSRF-TOKEN");
  if (!cToken || !cToken.value) {
    return NextResponse.json({ error: "Missing CSRF cookie" }, { status: 403 });
  }

  // token from headers
  const csrfToken = request.headers.get("X-XSRF-TOKEN");
  if (!csrfToken) {
    return NextResponse.json(
      { error: "Missing CSRF token in header" },
      { status: 403 }
    );
  }

  // verify if tokens are the same
  if (!csrfToken || csrfToken !== cToken.value) {
    return NextResponse.json({ error: "CSRF token mismatch" }, { status: 403 });
  }
  
  // verify if token is valid
  if (!tokens.verify(secret, csrfToken)) {
    return NextResponse.json({ error: "Invalid CSRF token" }, { status: 403 });
  }
  
  const response = NextResponse.json({ message: "TOKEN_VALID" });
  return response;
}
```

4. Now We can define final action - it is delegated to function savePost in file: src/app/posts/actions.ts.
We use zod library for validation of data in form:

{% include code-header.html %}
```js
"use server";

import {
  fetchClient,
  generatePostTag,
  updateClient,
} from "@/common/clientApi/fetchClient";
import { FormState, Post } from "@/types/Posts";
import { revalidatePath, revalidateTag } from "next/cache";
import { redirect } from "next/navigation";
import { z } from "zod";

// definition of validation object created with zod library
const Validation = z.object({
  title: z.string().min(3),
  body: z.string().min(3),
  tags: z.string(),
});

export type PostData = z.infer<typeof Validation>; // get & export type from zod object "Validation" with function zod infer

// add +1 to reactions field of specific post
export const likePost = async (postId: number) => {
  const post = await fetchClient<Post>(`http://localhost:3004/posts/${postId}`); // get Post object from API
  await updateClient("PATCH", `http://localhost:3004/posts/${postId}`, {
    // send to API path for this object with reactions +1
    reactions: post.reactions + 1,
  });
  revalidateTag(generatePostTag(postId)); // update cached values with specific tag [all values with this tags even on different paths] - so page refresh is automatically done on function likePost call
  // revalidatePath(`/posts/${postId}`) // update cached values on whole path [all values no mettr what tags are but on specific path]- so page refresh is automatically done on function likePost call
};

// save new or edit existing post
export const savePost = async (state: FormState, formData: FormData) => {
  // dynamic mapping of fields from form => create  object data with properties like fields in form
  const data: { [key: string]: unknown } = {};
  for (const pair of formData.entries()) {
    data[pair[0]] = pair[1];
  }

  // validate fields
  const parseResult = Validation.safeParse(data);
  if (parseResult.success === false) {
    const newState: FormState = {
      errors: parseResult.error.flatten().fieldErrors,
    };
    return newState;
  }

  const id = Number(formData.get("id"));

  const dataToSend = {
    ...parseResult.data,
    tags: parseResult.data.tags
      ? (parseResult.data.tags as string).split(",")
      : [],
    reactions: id ? undefined : 0, 
    // in PATCH we send undefined, so PATH will not modify reactions value, 
    // otherwise send 0 for new Post
  };

  await updateClient(
    id ? "PATCH" : "POST",
    `http://localhost:3004/posts${id ? `/${id}` : ""}`,
    //   {
    //   title:formData.get("title")
    //   //  etc. for every other field from form
    // }
    dataToSend // use data mapped before
  );

  revalidatePath("/posts");
  if (id) {
    revalidateTag(generatePostTag(id));
  }
  redirect("/posts");
};
```

5. Function that update is called updateClient that is in file: src/common/clietApi/fetchClient.ts.

{% include code-header.html %}
```js
import { notFound } from "next/navigation";

type FetchClientOptions = {
  // in options client will get revalidate param (sec set for cache)
  revalidate?: number;
  tags?: string[];
};

export async function fetchClient<P = unknown>( // using Generic <P=unknown> We will be able to use client with all types of data
  url: string,
  options: FetchClientOptions = { revalidate: 10 }
) {
  const { revalidate, tags } = options; // destructure - take number value from options
  const responseData = await fetch(url, { next: { revalidate, tags } });
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

That's all!

*INFO: This post is my notes based on Udemy course ("Nextjs 14 od podstaw") by Jarosław Juszkiewicz