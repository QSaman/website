---
title: LaTex
taxonomy:
    category: docs
---

### Convert LaTex to PDF

```
$ pdflatex foo.tex
```

### Convert LaTex to epub

For more information visit read [this](https://tex.stackexchange.com/a/41402). You need to convert the `tex` file into `xml` and then `html` and finally `epub`:

```
$ latexml --dest=foo.xml foo.tex
$ latexmlpost --dest=foo.html foo.xml
$ ebook-convert foo.html foo.epub --language en --no-default-epub-cover
```
