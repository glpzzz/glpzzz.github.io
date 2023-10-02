---
title: "Updating my website with htmx"
date: 2023-09-29
description: "How do I applied a very simple enhancement to my website using the boost feature of htmx?"
image: https://htmx.org/img/memes/bellcurve2.png
---

I have been reading a lot about htmx.org on Twitter lately and also seen an increase of the videos related to it on
Youtube.

## What is htmx?

Basically it is a JavaScript library to extend HTML by adding functionality including certain attributes in the HTML
markup.

There is a lot to say about it but I'll refer you to the 2 more important sources

- [htmx.org](https://htmx.org) the official documentation
- [Hypermedia Systems](https://hypermedia.systems/) a book on the hypermedia concept (the basics of htmx) and a very
  good resource of simple (not basic) web development.

## Why to use htmx?

1. You want to enhance the user experience of your website.
2. You don't want to setup a project with React/Angular/Vue or any other framework.
3. You want to do it fast.

## How to (quickly) use htmx?

I'll use this website as an example.

- By today (September 2023) it is an static website created using Jekyll.
- When you click a link the whole page is reloaded
- There are common elements that could be kept (the head tag, the footer)
- All the pages are using `_layouts/home.html` as the layout

### hx-boost

From the official [documentation](https://htmx.org/attributes/hx-boost/):

> The hx-boost attribute allows you to “boost” normal anchors and form tags to use AJAX instead. This has the nice
> fallback that, if the user does not have javascript enabled, the site will continue to work.

Now the links, instead of triggering a regular GET request to the server, trigger an AJAX request and the content of
the `body` tag is the only thing refreshed when the response is received. All the common assets (css, js, fonts, images)
are already loaded so

- the loading time is reduced
- the user experience is improved
- there is not flicker when the content changes
- the viewport automatically scrolls up when the new content is loaded

**But there is a catch:** the tags in the `head` are not updated. Title, description, OpenGraph nor Schema.org. How to
solve?

### The head-support Extension

Again from the official [documentation](https://htmx.org/extensions/head-support/#usage):

> If the htmx request is from a boosted element, then the following merge algorithm is used:
> - Elements that exist in the current head as exact textual matches will be left in place
> - Elements that do not exist in the current head will be added at the end of the head tag
> - Elements that exist in the current head, but not in the new head will be removed from the head

As a result just the desired tags in `head` are updated (title, description, OpenGraph, Schema.org) and the rest stays
the same.

### The actual changes

Just two things were required:

**Step 1:** Add the htmx and extension to the layout (`_layouts/home.html`) of all the pages:

```html
<script src="https://unpkg.com/htmx.org@1.9.5"
        integrity="sha384-xcuj3WpfgjlKF+FXhSQFQ0ZNr39ln+hwjN3npfM9VBnUskLolQAcN80McRIVOPuO"
        crossorigin="anonymous"></script>
<script src="https://unpkg.com/htmx.org/dist/ext/head-support.js"></script>
```

**Step 2:** dd the hx attributes to the body tag. Note that I used the `data-` prefix for the __hx__ attributes in order
to prevent issues with HTML validators

```html
<body class="markdown-body" data-hx-boost="true" data-hx-ext="head-support">
``` 

## Conclusion

With this very simple change, the navigation and reading experience is way better.

Also, as stated before, if the user don't have JavaScript enabled, the site will continue to work with regular requests.

We are loading new content using AJAX without writing a single JavaScript line.

There is a lot more to say about htmx but this is enough to get started.