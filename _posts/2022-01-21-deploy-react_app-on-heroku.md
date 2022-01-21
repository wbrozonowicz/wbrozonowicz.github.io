---
title: "React app on Heroku"
excerpt: "How to deploy React app on Heroku"
last_modified_at: 2022-01-21T10:27:01-05:00
categories:
  - JavaScript
tags: 
  - React
  - Deploy
  - Heroku
---

<!-- short introduction -->
## React deploy on Heroku

Deploy React application on Heroku is extremely easy:
1. create new app on Heroku dashboard webpage
2. choose option Automatic deploys from github repo and provide your repo name
3. go to Your app settings and click on Buildpacks section "Add buildpack"
4. add react buildpack: "https://buildpack-registry.s3.amazonaws.com/buildpacks/mars/create-react-app.tgz" and save changes

![Add React buildpack](/images/react_posts/react_buildpack.png)

That's all - with next git commit Your React app will be build automatically by Heroku!

