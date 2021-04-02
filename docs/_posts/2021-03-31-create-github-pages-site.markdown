---
layout: post
title:  "Creating a GiHub Pages site for your project with Jekyll"
date:   2021-03-31 -0400
category: web-development
tags: github jekyll
---

I am going slightly off topic here ;) This is how I set up this website using Github Pages and Jekyll. This article assumes you have basic working knowledge of Git and Command Line.

My system: Raspeberry Pi 400 with Raspbian 5.10.11

* Table of Contents
{:toc}

# Create a repository on Github

First things first, follow the [official Github documentation](https://docs.github.com/en/github/working-with-github-pages/creating-a-github-pages-site-with-jekyll) and create a repository for your site. 

Since we're creating a website for a project instead of a user site, we do not need to follow the naming convention specified in the documentation.

Remember to initialize the project repository with README and .gitignore (python template) files.

# Install Jekyll 

Install Jekyll by following the [official installation instructions](https://jekyllrb.com/docs/installation/). 

I took the [Ubuntu](https://jekyllrb.com/docs/installation/ubuntu/) route, your installation and dependencies may differ depending on your operating system.

# Clone to a local Git repository

Moving forward, we're now at the [creating-your-site](https://docs.github.com/en/github/working-with-github-pages/creating-a-github-pages-site-with-jekyll#creating-your-site) step but with a small adjustment.

Because we set up the project repository on Github in previous step, we will clone the remote repo to our local folder. 

**Navigate to the location where you want to store your repository locally. Let's call it PARENT-FOLDER.**

Open Terminal, run

````
$ cd PARENT-FOLDER
````

**Clone the project repository to your local folder.**

````
$ git clone PROJECT-REPOSITORY-URL
````

Now you should have the repo folder in the parent-folder.

**Navigate to the repository.**

````
$ cd PROJECT-REPOSITORY
````

Tip: if the name of the folder contains space, use quotation marks

````
$ cd "FOLDER NAME"
```

# Create a new website with Jekyll

I chose to publish my website from the docs folder on the default branch (main branch). You can also choose to create a new gh-pages branch and publish the website from there, check out the [Github documentation](https://docs.github.com/en/github/working-with-github-pages/creating-a-github-pages-site-with-jekyll#creating-your-site).

To create a folder named docs, and change directories to docs:

```
$ mkdir docs
$ cd docs
```

Now the fun part begins. We're going to create a new Jekyll site inside docs. To do this, run:

```
$ jekyll new .
```

And follow the [Github documentation](https://docs.github.com/en/github/working-with-github-pages/creating-a-github-pages-site-with-jekyll#creating-your-site) from step 8 to step 12.

# Modify _config.yml and preview site locally

We can already preview our site locally with Jekyll, but without editing _config.yml it might not show up correctly once we publish it to Github. More on that later, let's check out our brand new site.

To view the site locally, in Terminal, inside the docs directory, run:

```
$ bundle exec jekyll serve
```

Open browser, type "localhost:4000" to the address, you should see the site up and running.

Now let's edit the _config.yml. There are several places you can and should customize:

* title
* email
* description
* baseurl
* url

The last two items are actually very important in making sure Github renders the site correctly. I commented out baseurl, and added "https://username.github.io/project-repository" as url.


# Publish website on Github

Push the repository to Github, we can run standard git commands in Terminal such as:

```
$ git status
$ git add .
$ git commit -m "commit message"
$ git push
```

Because we only have one branch (the main branch), there is no need to switch branches and what not.

Now let's go to Github and [choose a publishing source](https://docs.github.com/en/github/working-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site#choosing-a-publishing-source). In our case, we choose main branch and docs folder as our source. After save, we now have our site's URL "https://username.github.io/project-repository".

Navigate to the URL, and we have a shiny new website!
