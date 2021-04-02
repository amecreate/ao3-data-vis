---
layout: post
title:  "Adding Tags on A Jekyll Site"
date:   2021-04-02 -0400
category: website
tags: jekyll html
---

# Adding Tags to Posts

Jekyll allows adding tags in the [front matter](https://jekyllrb.com/docs/front-matter/), however to display the tags on the post itself we need to write some simply HTML and Liquid.

Firstly, we need to modify `_layouts/post.html`.