---
title: About
permalink: /about/
layout: page
---
This site serves as the Wiki for the City of Zanesville GIS processes and procedures.

The site is a [Jekyll](https://docs.github.com/en/github/working-with-github-pages/setting-up-a-github-pages-site-with-jekyll) template, served by GitHub Pages. It uses Spectre CSS for the main styling, Tocbot for the heading menus and highlight.js for syntax highlighting.

Posts are found in the ``_posts`` folder, with the default category of ``documents``.

The category pages are single pages with no content found under ``_categories``. 

### Categories
- Apps
- Documents
- Tasks
- X-Archive

You can run and edit the wiki locally with ``npm start``, assuming Ruby and the Jekyll gem are installed on your computer. Instructions can be found on the [Jekyll website](https://jekyllrb.com/docs/installation/windows/) on how to install Jekyll.

You can edit it locally by running ``npm start`` and using the Edit this Page link, or by visitng [http://localhost:4000/admin/](http://localhost:4000/admin/).

---

### Commands
```JavsScript
{
  "clean": "jekyll clean",
  "start": "jekyll serve --baseurl=''",
  "build": "jekyll build"
}
```