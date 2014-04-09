Slides
======

My slide collection, in Markdown and for reveal.js

To build, download or clone from
[reveal.js](https://github.com/hakimel/reveal.js.git) into the slides folder.

Install the latest [pandoc](http://johnmacfarlane.net/pandoc/).

Run:

```bash
    pandoc -r markdown -w revealjs -V theme=moon -s $slides.md > $slides.html
```
