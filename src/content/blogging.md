+++
author = "David Evans"
draft = false
title = "Blogging Mechanics"
slug = "blogging"
+++

Here are some suggestions for how to create the class blog posts for
your assigned classes.  I believe each team has at least a few members
with enough experience using git and web contruction tools that
following these instructions won't be a big burden, but if you have
other ways you want to build your blog page for a topic let me know
and we can discuss alternative options.

- Install [Hugo](https://gohugo.io/).  Hugo is a static website
  generator that builds a site from Markdown pages.  (With homebrew on
  Mac OS X, this is easy: `brew update && brew install hugo`.)

- Clone the github repository,
  [_https://github.com/secML/secML.github.io_](https://github.com/secML/secML.github.io).
  This is what is used to build the
  [https://secml.github.io/](https://secml.github.io/) site.  If you are
  working with multiple teammates on the blog post (which you probably
  should be), you can add write permissions for everyone to the cloned
  repository.

- You should create your page in the `web/content/post/`
  subdirectory. You can start by copying an earlier file in that
  directory (e.g., `class1.md`) and updating the header section
  (between the `+++` marks) and replacing everything after that with
  your content.  Don't forget to **update the date** so your page will
  appear in the right order. You can put `draft = true` in the header,
  so your page will not appear on the course website until it is
  ready.

- You can use multiple files (but probably only one in
  the `post/` directory (this will show up as pages on the front
  list).  Use the `web/content/images` directory for images and the
  `web/content/docs` directory for papers.  Using images and other
  resources to make your post interesting and visually compelling is
  highly encouraged!
   
- Write the blog page using Markdown.  Markdown is a simple markup
  language that can be used to easily generate both HTML and other
  output document formats.  You can probably figure out everything you
  need by looking at previous posts, but for a summary of Markdown,
  see [Markdown: Syntax](https://daringfireball.net/projects/markdown/syntax).

- To test the post, run `make develop` (in the `web/` subdirectory of
  your repository).  This starts the Hugo development server, usually
  on port 1313 (unless that port is already in use).  Then, you can
  view the site with a browser at `localhost:1313`.

- When you are ready, submit a pull request to incorporate your
  changes into the main repository (and public course website).  Also,
  send a message to me (dave) on slack, so I know the post is ready to
  review.  At this stage, I will probably make some requests for
  improvements, and then will post the edited version to the course
  site.