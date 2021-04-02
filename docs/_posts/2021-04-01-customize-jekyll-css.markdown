---
layout: post
title:  "CSS Customization with Jekyll and GitHub Pages"
date:   2021-04-01 -0400
category: web-development
tags: jekyll css
---

Some minor customizations on the appearance of the website.

Table of Contents

* Table of Contents
{:toc}

# File Structure

This is [an example](https://github.com/jekyll/jekyll-sass-converter/tree/master/docs) of how your local folder (and GitHub repository) should look like in terms of file structure.

- Root
   - assets
      - css
        - main.scss
   - _sass
    
   - _posts
   - _site

Specifically, in my case, on a default Jekyll minima site created locally (root folder `docs`), I need to:

1. In `docs/assets`, create a new folder named `css`.
2. Navigate to `gems/gems/minima-2.5.1` folder
3. Copy the file `assets/main.scss` to `docs/assets/css` 
3. Copy the folder `_sass` to `docs`.

# Change Jekyll Code Block Background Colour

The default colour for code formatting is ![#eeeeff](https://via.placeholder.com/15/eeeeff/00000?text=+) `#eef`. 

To change this colour, we need to edit two files:

1. `_sass/minima/_syntax-highlighting.scss`
2. `_sass/minima/_base.scss`

In file 1, change the following 

```
  .highlighter-rouge & {
    background: #eef;
```

to your desired colour. I used the default light grey colour defined in `minima.scss` file.

```
  .highlighter-rouge & {
    background: $grey-color-light;
```

In file 2, under `Code formatting`

delete `background-color: #eef;` from

```
pre,
code {
  @include relative-font-size(0.9375);
  border: 1px solid $grey-color-light;
  border-radius: 3px;
  background-color: #eef;
}

```

add your own colour code like this

```
pre {
  padding: 8px 12px;
  overflow-x: auto;
  background: $grey-color-light;
```

To preview the change locally on your machine, save all files, make a draft post with some code blocks, and run Terminal command inside root folder:

```
$ bundle exec jekyll serve
```

To publish to GitHub Pages, push all above files and folders to your remote publishing repository.

# Change Table Width on a Jekyll site

I write Python in Jupyter Lab and convert the notebook to markdown, the tables in my markdown file look like this

```
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
...
```

In my case, it is pretty simple to change the table width with a little CSS. 
In `_sass/minima/_layout.scss`, add your customization

```
.dataframe { 
  table-layout: fixed;
  width: $content-width;
  
  td {
    overflow:hidden;
  }
}
```

Again, the `$content-width` is defined in `minima.scss` file.

Lastly, push all changes to the remote repository.