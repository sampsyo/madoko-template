[INCLUDE="style"]

madoko-template
===============

[Madoko][] is a wonderful Markdown-based scholarly writing system by [Daan Leijen][].
This repository contains a few boilerplate files that I use when starting any document with Madoko.
It includes a Makefile for building PDF and HTML versions of your document and a style preamble with reasonable defaults for more sensible and beautiful output.

[madoko]: https://www.madoko.net
[Daan Leijen]: https://www.microsoft.com/en-us/research/people/daan/


## Set Up

To start a Madoko project, you'll want to install Madoko:

    $ npm install -g madoko

Then, copy the files from this repository into your project directory.
You'll at least want `madoko.mk` and `style.mdk` and probably `.gitignore`; the rest you can create yourself.

Edit your `Makefile`. At a minimum, it should look something like this:

    TARGETS := mydoc
    DEPS := style.mdk
    include madoko.mk

The `TARGETS` line should indicate your `*.md` Markdown file where you'll do your writing. You can list multiple documents there if you want to produce multiple, separate documents.
Use the `DEPS` variable to list other files that your document depends on, such as figures (and the included style preamble).

In your Markdown source file, add this line to use the included style preamble:

    [INCLUDE="style"]

You'll want to put that in your Madoko "header" along with any the other metadata declarations.


## Make Targets

The `madoko.mk` Makefile fragment includes these targets for building and viewing your document:

* `make`: Build a PDF of your document as `build/*.pdf`. Madoko also builds an HTML version (there's no way to disable this, as far as I can tell).
* `make html`: Build just the HTML version as `build/*.html`.
* `make view`: Build the PDF and open it for viewing.
* `make view-html`: Open the built HTML page in your browser.

It can also automatically rebuild your document when source files change if you have [liveserve][] installed:

* `make watch`: Automatically rebuild the PDF when source files change.
* `make watch-html`: Automatically rebuild the HTML version and reload it in any open browsers.

It can also upload the result to a server for sharing. This can be particularly useful with [Hooknook][] for automatic deploys.
Add a `DEST` variable with your `scp`-style destination path to your Makefile:

    DEST := ssh.example.com:/some/path

and then use these targets:

* `make deploy`: Build a PDF and copy it to your server.
* `make deploy-html`: The same, except for Madoko's HTML output.

There's also a `make clean` target, naturally.

[liveserve]: https://github.com/sampsyo/liveserve
[hooknook]: https://github.com/sampsyo/liveserve


## Style

The included `style.mdk` preamble does a few things that look good to me:

* Use a simple, numerical citation format by default.
* Madoko's syntax highlighting does not use color in the PDF output. To me, colorized source code is fine on a Web page but looks silly on paper.
* Similarly, links are not colorized in the PDF output. That's just wrong.
* Make the "todo" environment in Madoko source show up red. You can also use `&tk;` to insert a simple red placeholder with no explanatory text.
* Use Adobe's [Source Code Pro][] font for code in the PDF output, which I think looks better than the TeX default.
* The Web output uses in-browser hyphenation, a nice Palatino-based font stack, less harsh link colors, and looser spacing. Here's [an example output][ppl].

[Source Code Pro]: https://adobe-fonts.github.io/source-code-pro/
[ppl]: http://adriansampson.net/doc/ppl.html


## Author

By [Adrian Sampson][adrian], with apologies to [Ben Ransford][ben] whose [pdflatex-makefile][] inspired this (while being a hundred times more impressive and more useful).

[adrian]: http://www.cs.cornell.edu/~asampson/
[ben]: http://ben.ransford.org
[pdflatex-makefile]: https://github.com/ransford/pdflatex-makefile
