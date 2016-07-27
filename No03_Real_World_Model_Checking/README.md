# Slides for the presentation "Real World Model Checking Using SAT Solvers"

The slides are included both in source form (Markdown) and PDF for convenience.

The markdown file is designed to be used with [Reveal.js](https://github.com/hakimel/reveal.js).
The whole reveal.js project is not included here to avoid cluttering the folder.
In case you'd like to recreate the reveal.js project, clone the reveal.js
repository and edit `index.html` so that the main `<div>` looks like this:

```
<div class="reveal">
	<div class="slides">
		<section
			data-markdown="presentation.md"
			data-separator="\n::\n"
			data-separator-vertical="\n%%\n"
			data-separator-notes="^Note:"
			data-charset="utf-8"
		></section>
	</div>
</div>
```

you can now start a local HTTP server from the reveal.js folder:

```
python -mSimpleHTTPServer 9090
```

and navigate to [http://127.0.0.1:9090/](http://127.0.0.1:9090/).
