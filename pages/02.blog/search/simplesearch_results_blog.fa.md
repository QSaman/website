---
title: Blog Search results
content:
    items:
        '@taxonomy':
            category: [fa_blog]
    order:
        by: title
        dir: desc
simplesearch:
     route: @self
     filters:
         - @self
     filter_combinator: and
---

