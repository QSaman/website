---
title: Blog
sitemap:
    changefreq: monthly
body_classes: 'header-dark header-transparent'
hero_classes: 'text-light title-h1h2 overlay-dark-gradient hero-large parallax'
custom: 'new thing'
blog_url: /blog
show_sidebar: true
show_breadcrumbs: true
show_pagination: true
content:
    items:
        '@taxonomy':
            category: [en_blog]
    limit: 6
    order:
        by: date
        dir: desc
    pagination: true
    url_taxonomy_filters: true
feed:
    description: 'Sample Blog Description'
    limit: 10
pagination: true
hero_image: user://pages/files/images/my-header.jpg
simplesearch:
    route: /blog/search
    filters:
        - @self
        - @taxonomy: [en_tag]
    filter_combinator: and
---


## Blog

#### Welcome to my personal blog. For tutorial please visit this [page](/tutorial/notes).
