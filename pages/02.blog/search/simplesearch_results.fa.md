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

<div class="search-wrapper">
  <a href="{{ base_url }}/blog/search"><i class="fa fa-times-circle"></i></a>
  <input type="text" placeholder="Enter keywords to search..." value="{{ query }}" data-search-input="{{ base_url }}/blog/search/query" />
</div>
