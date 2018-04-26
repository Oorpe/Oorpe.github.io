---
title: getting up to speed with Jekyll
date: 2018-04-26 04:39:46 -0400
category: programming
title: "Blogging with Jekyll"
description: "Getting up to speed with a new tech is always more interesting than doing what you're supposed to"
draft: true
---

## getting up to speed with Jekyll

I've never been very interested with blogging.

This spring something's come over me, and I've been writing a lot in a very short amount of time. So I figured that I should finally get started on building some kind of blog to post some of this outpouring of written word. I'd been eying github pages as a blogging platform for a while, so this seemed as good a time as any to get started on some kind of a web presence.

### How do you- ?

So, at first I created the repo for my profile blog. I selected a theme, wrote little snippets of code and tried posting them. Then I lost interest and dropped the thing for a month.

Only after realizing I'm a few months late on my thesis and desperately looking around for ways to procrastinate instead of working on it, I came across the blog repo and found a real font of wasting time I should be using on getting my studies done in a reasonable time. So I installed the required gems for local development and got to researching jekyll.

As a start, I got going by leafing through github pages' article on blogging with Jekyll and set up the project.

```
    |- _includes
    |- _layouts
        |- main.html
        |- post.html
    _posts
```

I decided I wanted to use some sweet, sweet material design for the site's styling, instead of one of the default themes that come with the `github-pages` ruby gem. I originally opted for MDL, the reference implementation of Material Design that is fairly framework-agnostic and lightweight.

### Styling

MDL is neat in that it has a set of prebuilt color schemed CSS files in it's CDN, allowing me to just specify the primary-secondary colors I want to use in my site via the link tag: `<link rel="stylesheet" href="https://code.getmdl.io/1.3.0/material.[ primary + '-' + accent ].min.css">`. After building the site with thematic colors via `mdl-primary` and `mdl-accent` classes, I can change the colors represented by the keywords by just downloading a different stylesheet.

### Categories

I also wanted to have a rudimentary category system. This isn't directly built in to Jekyll, so it took a moment to cobble the functionality together, especially since the system wasn't exactly familiar to me beforehand.

Jekyll's post system is based around a Rails-y concept of convention over configuration. Every post in the _posts directory is treated as a post as long as it's name follows a certain naming scheme- "YYYY-MM-DD-name-of-post" + file suffix. In liquid templating, jekyll presents a bunch of variables to be used inside the templates, as well as logical structures to be run at build time. This allows for constructing pretty elaborate use cases, like a categorical way to construct navigation buttons.

### Creating some includes

Well, since this is a templating language, let's define some pretty templates for common functionality.

First we'll need a nice link to a post. I'd decided to have a basic description field in the yaml front matters of all of my posts, for displaying little blurbs of the post. These were a natural way to prettify my links, adding tooltips that would give a quick blurb about the contents of the post being linked. The include file itself is a basic liquid include, which is passed one to three parameters: `post`, the post object to link to, `p_prev` which determines whether this link points to a post in the past, and `p_next` which determines if this link points to a post in the future.

{%raw%}
```html
    <div class="mdl-tooltip" data-mdl-for="{{include.post.url}}">
        {{include.post.description | default: include.post.title | truncate: 52}}
    </div>
    <a id="{{include.post.url}}" href="{{ include.post.url }}"
        class="mdl-button mdl-button--accent">
      {% if include.p_prev == "true" %}{% assign which = "prev: "%}{%endif%}
      {% if include.p_next == "true" %}{% assign which = "next: "%}{%endif%}
      {% capture title %}{{which}}{{include.post.title}}{%endcapture%}
      <small>{{include.post.date | date_to_string}}</small>
      {{title | truncate: 32}}
    </a>
```
{%endraw%}

Using MDL, I added tooltips and button styling to the links, as well as basic truncation to the tooltip texts and the title length in order to keep the total text lengths sane.

This file is placed in the directory `_includes`, and included every time I link to a post from another post, a menu or an archive page.
