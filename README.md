# Markdown Formatting Utilities

This repository contains two utilities for formatting Markdown documentation. Unfortunately a number of Git servers don't support the notion of a table of contents nor does Markdown number sections, both highly desirable with large amounts of documentation.

In short `renumber-files` is a basic script for renumbering chapter files when new chapters or sections have been added (see [restructuring](#restructuring)) and `generate-markdown-book` does the work of building the final Markdown document (see [generating books](#generating-markdown-books)).

## Restructuring Chapters <a name="restructuring" id="restructuring"></a>

When writing large Markdown documents it's advisable to break down the documentation into individual chapters or even sections for very large chapters. One may be inclined to leave it at that, however:
- Some Markdown renderers can't cope with references that cross file boundaries.
- When presented with the entire document as one page inside a web browser then the user can easily search it using the browser's search tool. If the document is split up into separate chapters then you could only search a chapter at a time or need to implement a search function on the web site.

`renumber-files` is a simple script and assumes that each chapter is named `Chapter*CCSS*-*<Chapter/Section Title>*.md` where *CC* is the chapter number and *SS* is the section number. For most chapters it probably isn't worth splitting it out into separate sections and so the section number is just left at 01, it doesn't mean that there's only one section in that chapter!

### Renumbering Chapter Files

In the event you add a new chapter or section file then:

- Temporarily rename it so that it fits in the right order when you do `ls` (by prefixing the chapter or title with something).

- Use the `renumber-files` script to rebalance the numbering.

- Remove the prefix you added earlier.

For example let's say you wanted to add a section you'd saved in `Image-Touchup-Tools.md` to Chapter 8 after section 11, you would do something like this:

```shell
mv Image-Touchup-Tools.md Chapter0811-ZZImage-Touchup-Tools.md
git add Chapter0811-ZZImage-Touchup-Tools.md
./git-renumber-files Chapter0*
git mv Chapter0812-ZZImage-Touchup-Tools.md Chapter0812-Image-Touchup-Tools.md
git commit -am "Add the Image Touchup Tools section"
```

## Generating Markdown Books <a name="generating-markdown-books" id="generating-markdown-books"></a>

Assuming you've stored your chapters and sections in a subdirectory in the repository, to generate the user guide simply do:

```shell
./generate-markdown-book Chapter*.md >| ../README.md
```

This will generate one book which will be version stamped and contain a proper table of contents that will actually work with BitBucket and other Git servers. This program will:

- Change each section to include a section number and a reference id based on the section name that uses HTML and should be compatible with any Git server.

- Replace the `[date]` token with today's date in the form of *DD/MM/YYYY*.

- Replace the `[toc]` token with a proper table of contents. This must be on a line on its own.

- Replace the `[version]` token with the version taken from `VERSION.txt`.

### Editing

Some points when editing the original MarkDown files:

- There's no need to number sections, that will be done for you. Just use `##` in the usual way. Start with `##` for chapters and `###` for sections and so on down the hierarchy. Only use `#` for the main title of the document.

- Each section will have a reference id generated for it based upon the section name in lower case and `-` substituted for any non-alphanumeric character. So to refer to another section titled `My Favourite Food` you'd use `[my favourite food](#my-favourite-food)`. Only if there are duplicate titles do they get prepended with the section number(s), making it more brittle to change. `generate-markdown-book` will warn you about this.

- I'm afraid the best way to check correct MarkDown formatting is to check the result into Git and view it in BitBucket. VS Code's MarkDown viewer is pretty good but there are still some annoying discrepancies none the less. Once you're happy then ***PLEASE*** use `git rebase` (to rewrite history) or `git reset` (to simply rewind your commit history without changing the workspace) in order to collapse your commit history down to something sensible and then do a forced push to the Git server.

Thank you. :-)
