---
title:  "Meta: Jekyll with tags and categories"
date:   2016-12-15 08:00:00 +0000
modified: 2016-12-15 08:00:00 +0000 
comments: true
permalink: /weblog/2016/12/15/jekyll-tags-categories/
categories: blog howto
tags: blog meta jekyll howto
---

Once a decent number of blog posts and pages on a single domain is reached, it becomes necessary to sort and categorize single posts. That way sequential articles can be written or information on a common topic be grouped. Since GitHub Pages do not support custom Jekyll plugins a little manual work is needed.

<!--more-->

You could either just copy my [blog][blog] or try to follow the necessary steps on your installation.


# Contents

1. Introduction
1. Configuration
1. Tags
1. Categories
1. *post* layout update
{:toc}


---

## Introduction

When having the following approach applied to your blog, you will be able to omit the `layout` definition on each and every page or post. The reason is, you'll be managing the default layouts from the central configuration file. Once everything is configured correctly, the following post style can be used.

```
---
title:  "This is another test"
date:   2017-01-01 08:00:00 +0000
modified: 2017-01-01 08:00:00 +0000
permalink: /2017/01/01/another-test/
categories: web documentation 
tags: meta howto css html javascript
---

Teaser

<!--more-->

Even more information
```

{% include tags/hint-start.html %}
Think about a methodical usage of *categories* and *tags*. Try to select them in a non-mixable way to minimize redundancy.
{% include tags/hint-end.html %}

This will be the desired directory listing if you'd want to mirror mine:

```
.
user.github.io
├──[_drafts]
├──[_includes]
│   ├── tagcloud.html
│   └── categorylist.html
├──[_layouts]
│   ├── default.html
│   ├── post.html
│   ├── page.html
│   ├── posts_by_category.html
│   └── posts_by_tag.html
├──[_meta_categories]
│   ├── web.html
│   └── documentation.html
├──[_meta_tags]
│   ├── meta.md
│   ├── howto.md
│   ├── css.md
│   ├── html.md
│   └── javascript.html
├──[_posts]
│   └── 2017-01-01-another-text.html
├──[css]
├──[images]
├──[pages]
├── _config.yml
├── Gemfile
└── index.html
```

Alright, let's get started...


---


## Configuration

Open the `_config.yml` file and **add** the following lines, if they weren't existing yet.

```
# Location of collections
collections:
  meta_tags:
    output: true
    permalink: /tag/:name/
  meta_categories:
    output: true
    permalink: /category/:name/

defaults:
  -
    scope:
      path: ""
      type: pages
    values:
      layout: page
  -
    scope:
      path: ""
      type: posts
    values:
      layout: post
  -
    scope:
      path: ""
      type: meta_tags
    values:
      layout: posts_by_tag
  -
    scope:
      path: ""
      type: meta_categories
    values:
      layout: posts_by_category
```

Definitions:

 - *tag* pages generated from `meta_tags` are written and collected for queries
 - *category* pages generated from `meta_categories` are written and collected for queries
 - default layout for pages in the `pages` directory will be `page`
 - default layout for pages in the `posts` directory will be `post`
 - default layout for pages in the `meta_tags` directory will be `post_by_tags`
 - default layout for pages in the `meta_categories` directory will be `post_by_category`


{% include tags/hint-start.html %}
Default collections like `posts` and custom collections like `meta_tags` or `meta_categories` are required to have an underscore prefix in the source directory. `pages` is not such a collection by default.
{% include tags/hint-end.html %}

---

## Tags

### Single tag

To create a new accessible tag (e.g. *HTML*) - simply add a new file for each. Here is how it looks, `/_meta_tags/html.md`
```
---
slug: html
name: HTML
---
```


### *Tag cloud* include

This include will be used in the posts layout, as well as *tags overview* page. Create a new file `/_includes/tagcloud.html`

```html{% raw %}
{% capture site_tags %}{% for tag in site.tags %}{{ tag | first }}{% unless forloop.last %},{% endunless %}{% endfor %}{% endcapture %}
{% assign site_tags = site_tags | split: ',' %}

{% assign tag_count = 0 %}
{% for tag in site_tags %}
{% assign tag_count = tag_count | plus: site.tags[tag].size %}
{% endfor %}

{% for tag in tags %}
{% assign rel_tag_size = site.tags[tag].size | times: 4.0 | divided_by: tag_count | plus: 1 %}
<span style="white-space: nowrap; font-size: {{ rel_tag_size }}em; padding: 0.6em;">
	<a href="{{ site.baseurl }}/tag/{{ tag | slugize }}/" class="tag">{{ tag | slugize }} <span>({{ site.tags[tag].size }})</span>
	</a>
</span>
{% endfor %}
{% endraw %}```

Description:

 - At first an inline script determines the total number of tags being used on the blog. This will be useful to set a relative font sizes for a single tag.
 - In the latter part the script iterates over an **input variable** an array of tags called `tags`. It determines the relative font size and writes an linked `<span>` field.
 - *input variable:* `tags`

### *Posts by tag* layout

Create a new layout file `/_layouts/posts_by_tag.html` for a post listing of a single tag. The list is grouped by publication year.

```html{% raw %}
---
layout: default
---

<h1>Posts tagged '{{ page.name }}'</h1>

<div class="text-justify">
  {% if site.tags[page.slug] %}
    {% for post in site.tags[page.slug] %}
      {% capture post_year %}{{ post.date | date: '%Y' }}{% endcapture %}
      {% if forloop.first %}
        <h3 class="m-t-3 m-b-1">{{ post_year }}</h3>
        <div class="list-group">
      {% endif %}
      {% unless forloop.first %}
        {% assign previous_index = forloop.index0 | minus: 1 %}
        {% capture previous_post_year %}{{ site.tags[page.slug][previous_index].date | date: '%Y' }}{% endcapture %}
        {% if post_year != previous_post_year %}
          </div>
          <h3 class="m-t-3 m-b-1">{{ post_year }}</h3>
          <div class="list-group">
        {% endif %}
      {% endunless %}
      <a href="{{ post.url }}" class="list-group-item">
        <h4 class="list-group-item-heading h6">{{ post.title }}</h4>
      </a>
      {% if forloop.last %}
        </div>
      {% endif %}
    {% endfor %}
  {% else %}
    <p>There are no posts in this tag.</p>
  {% endif %}
</div>
{% endraw %}```

Description:

 - This layouts page features a `tag` with both `slug` and `name` as attributes.


### *Tags overview* page

Create a new page as `/pages/tags.md`. This will retrieve all tags available to the blog and runs the previously defined *tagcloud* include to render a tag cloud entry for each word.

```html{% raw %}
---
title: Tags
permalink: /tags/
---

<div class="home">
	<h1 class="page-heading">All tags</h1>

	<p class="post-meta" style="text-align: justify;">
	{% capture site_tags %}{% for tag in site.tags %}{{ tag | first }}{% unless forloop.last %},{% endunless %}{% endfor %}{% endcapture %}
	{% assign tags = site_tags | split:',' | sort %}
	{% include tagcloud.html %}
	</p>
</div>
{% endraw %}```


---

 
## Categories

### Single Category

To create a new accesible category (e.g. *HowTo*) - simply add a new file for each. Here is how it looks, `/_meta_categories/howto.md`

```
---
slug: howto
name: HowTo
description: Short tutorials on different topics in computer science.
---
```

As you'll see `description` is an additional value I added for demonstration purposes.

### *Category list* include

Create a new file `/_includes/categorylist.html` for a local post category list.

```html{% raw %}
{% if site.meta_categories %}
in 
  {% for c in categories %}
	{% for cat in site.meta_categories %}
	  {% if cat.slug == c %}

<span title="{{ cat.description }}">
	<a href="{{ site.baseurl }}/category/{{ c | slugize }}/" class="cat">{{ cat.name }}</a>
</span>

	  {% endif %}
	{% endfor %}
  {% endfor %}
{% endif %}
{% endraw %}```

Description:

 - It features a sequence of `<span>` fields with a tooltip description.
 - It iterates over the `meta_category` collection to find the matching entry by the slug identifier.
 - *input variable:* `categories`


### *Posts by category* layout
Create a new layout file `/_layouts/posts_by_category.html` for a post listing of a single category.

```html{% raw %}
---
layout: default
---

<h1>Posts in '{{ page.name }}'</h1>
<p class="post-meta">{{ page.description }}</p>

<div class="text-justify">
  {% if site.categories[page.slug] %}
    {% for post in site.categories[page.slug] %}
      {% capture post_year %}{{ post.date | date: '%Y' }}{% endcapture %}
      {% if forloop.first %}
        <h3 class="m-t-3 m-b-1">{{ post_year }}</h3>
        <div class="list-group">
      {% endif %}
      {% unless forloop.first %}
        {% assign previous_index = forloop.index0 | minus: 1 %}
        {% capture previous_post_year %}{{ site.categories[page.slug][previous_index].date | date: '%Y' }}{% endcapture %}
        {% if post_year != previous_post_year %}
          </div>
          <h3 class="m-t-3 m-b-1">{{ post_year }}</h3>
          <div class="list-group">
        {% endif %}
      {% endunless %}
      <a href="{{ post.url }}" class="list-group-item">
        <h4 class="list-group-item-heading h6">{{ post.title }}</h4>
      </a>
      {% if forloop.last %}
        </div>
      {% endif %}
    {% endfor %}
  {% else %}
    <p>There are no posts in this category.</p>
  {% endif %}
</div>
{% endraw %}```

Description:

 - This layouts page features a `category` with the attributes: `slug`, `name` and `description`.

## *post* layout update

To have your *post* pages use the new *category* and *tag* feature, you have to edit the `/_layouts/post.html` and add the following lines according to your presets:

```html{% raw %}
<p class="post-meta">
	{% assign categories = page.categories %}
	{% include categorylist.html %}
</p>
<p class="post-meta" style="text-align: justify;">
	Tags:
	{% assign tags = page.tags | sort %}
	{% include tagcloud.html %}
</p>
{% endraw %}```



As you will see, you are now able to enter new meta information like `category` or `tag` as you please.



[blog]: https://github.com/newtork/newtork.github.io