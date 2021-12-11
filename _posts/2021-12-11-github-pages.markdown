---
layout: post
title:  "Setting up Github Pages for a project using React"
date:   2021-12-11 12:23:00 -0500
categories: jekyll update
---

## Why?
[GitHub Pages] is an easy way to get a static website on the web.

[create-react-app] is an easy way to create a single-page React app.

Why not combine them to create the easiest React app of our lives?

## Step 1: React App Git Repository
First we can scaffold a React app using this one liner.


{% highlight bash %}
npx create-react-app --template typescript;
{% endhighlight  %}

Template argument is optional. Call me old-fashioned, but I prefer my language statically typed, so I'll be using the Typescript template.

`create-react-app` will automatically create a repository for us as well. (+1 for automation!)

All we have to do now is push what we have to GitHub server:
{% highlight bash %}
git remote add origin <repository.git URL>;
git push -u origin main;
{% endhighlight %}

## Step 2: GitHub Configuration
Now we can change GitHub repository settings to use GitHub pages.
I wanted the static files to be in a specific directory, so I will be putting my static files for GitHub Pages in /docs.

In Settings > Pages under Source:
- Branch: main
- Folder: /docs

Not the directory name I would have chosen, but GitHub doesn't give you too many options.

## Step 3: Tweak package.json
You can do a production build of your React app by running `npm run build`.
You'll notice that `react-script` will put the files in `build` directory, not in `docs`, which is not what we promised GitHub.

This can be changed by editing `package.json`.
Find where the `build` script is defined, and add `BUILD_PATH=docs` environment variable at the beginning of the command:

{% highlight bash %}
...
"scripts": {
  ...
  "build": "BUILD_PATH=docs react-scripts build",
  ...
},
...
{% endhighlight %}

Another thing you will notice is that all the links to generated files are broken in index.html.

This is because `create-react-app` expects the base URL to be the root domain URL.
However, the way GitHub Pages works for repositories, your base URL will actually be in `<repository name>`.

For example, index.html will look for the stylesheet at `/static/css/main.a617e044.chunk.css` when it is actually at `/<repository name>/static/css/main.a617e044.chunk.css`.

Thankfully the base URL can be changed easily with the PUBLIC_URL environment variable.
We will modify the same line we modified earlier (replace &lt;repository name> with your actual repository name):

{% highlight bash %}
...
"scripts": {
  ...
  "build": "BUILD_PATH=docs PUBLIC_URL=<repository name> react-scripts build",
  ...
},
...
{% endhighlight %}

Finally, do another build, then push it to your main branch.
You should be able to see the default `create-react-app` page by going to `<github username>.github.io/<repository name>` after a few minutes.

I'll leave the hard part of implementing the website as an exercise to the reader. :)
