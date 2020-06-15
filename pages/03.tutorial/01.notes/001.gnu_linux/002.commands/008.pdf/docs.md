---
title: Creating PDF
taxonomy:
    category: docs
---

* To create password-protected pdf you can use `qpdf`:
```
$ qpdf --encrypt user-password owner-password key-length --  input.pdf output.pdf
``` 
