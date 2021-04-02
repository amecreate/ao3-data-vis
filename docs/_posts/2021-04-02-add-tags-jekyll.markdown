---
layout: post
title:  "Adding Tags on A Jekyll Site"
date:   2021-04-02 -0400
category: web-development
tags: jekyll html
---

Jekyll allows adding tags in the [front matter](https://jekyllrb.com/docs/front-matter/), however to display the tags on the post itself we need to write some simply HTML and Liquid.

# Adding Tags to Posts

Firstly, if you haven't done so already, copy the `_layouts` folder from `gem` folder to your site's root folder. (Here is a quick reference for [Jekyll directory structure](https://jekyllrb.com/docs/structure/)).

Open `_layouts/post.html` with your text editor, add the following to the `header` section.

```
{% raw %}
  <span class="post-tags">Tags: 
    {% for tag in page.tags %}
     <a href="/tag/taglist">{{ tag }}&nbsp;</a>
    {% endfor %}
  </span>
{% endraw %}
```

There are three options here:

1. Simply add the name of the tags to posts
2. Add the tags, link to respective page showing all posts under the tag
3. Or what I did, link all tags to a single static page showing all tags and posts

# Creating A Tag Page

In case one day I want to go with option 2, I created a `tag` folder in the root folder, and added a blank `taglist.markdown` file inside. 

This is the entirety of the `taglist.markdown`. The code is a direct copy from [Jekyll documentation](https://jekyllrb.com/docs/posts/). Simple, eh?

```
{% raw %}
---
layout: page
---

{% for tag in site.tags %}
  <h3>{{ tag[0] }}</h3>
  <ul>
    {% for post in tag[1] %}
      <li><a href="{{ post.url }}">{{ post.title }}</a></li>
    {% endfor %}
  </ul>
{% endfor %}
{% endraw %}
```

Push everything to Github, and the site is good to go!