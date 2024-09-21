---
title: "Next.js - new project"
excerpt: "How to start new project with Next.js"
last_modified_at: 2023-01-22T10:27:01-05:00
categories:
  - tools
  - Next.js

tags: 
  - basics
---

<!-- short introduction -->
## Next.js!

Next.js -> new framework recommended instead of CRA (create react app)

Start new project:

{% include code-header.html %}
```
npx create-next-app@latest
```



Options:

{% include code-header.html %}
```
✔ What is your project named? … first-next-js-app
✔ Would you like to use TypeScript? … No / Yes -> YES!!
✔ Would you like to use ESLint? … No / Yes -> YES!!
✔ Would you like to use Tailwind CSS? … No / Yes -> depends on need: manual css or by tailwind
✔ Would you like to use `src/` directory? … No / Yes -> YES!! (better project organisation)
✔ Would you like to use App Router? (recommended) … No / Yes -> YES!! (App router is newer approach)
✔ Would you like to customize the default import alias (@/*)? … No / Yes -> NO!! (better overview when You see full import path)
```

Don't forget to instal in VSC helpful plugins: ESLINT  & PRETTIER


Run project:

{% include code-header.html %}
```
npm run dev
```

Shoud start on localhost:3000

