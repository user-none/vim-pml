A PML bundle for Vim.

This plugin adds the following functionality:

* context aware autocompletion (aka omni-complete)
* PML snippets for [SnipMate][s]
* generate a clickable table of contents for `<title>`s in a chapter
* `<sectX>` block folding

This README file is primarily concerned with how to install the plugin and its dependencies. For a guide on how to use it, check the [Authors wiki][wiki].

Installation
============

Install the pml.vim plugin with [Pathogen][] (source on [github][pg]).

Some of the functionality depends on other Vim plugins. Brief installation notes follow for each of these.

TextMate style snippets for PML
-------------------------------

Installing [SnipMate][s] will enable you to use TextMate-style snippets. This pml.vim bundle includes [useful snippets for working with PML files][snips].

Follow the installation instructions on the [SnipMate project page][s], or use the [github mirror of SnipMate][sg] to install with Pathogen.

Auto-generate pairs of XML tags with [RagTag][]
-----------------------------------------------

Install the [RagTag][] plugin to get smart tag completion. These commands are particularly useful:

    <C-X><Space>  create an inline tag from the word in front of the cursor
    <C-X><CR>     create a block level tag from the word in front of the cursor
    <C-X>/        insert the closing tag for the previous unpaired opening tag

Generate a table of contents per chapter
----------------------------------------

![Table of contents](https://github.com/nelstrom/vim-pml/raw/master/screenshots/table-of-contents.png)

To add this functionality, you will have to install [exuberant tags][exuberant] on your system, as well as adding the [taglist.vim][TagList] plugin to Vim.

On OS X, you can get exuberant tags by running: `brew install ctags`. On linux, you can install it by running: `sudo apt-get install exuberant-ctags`.

You can install the taglist.vim plugin the traditional way, by [following the instructions on the vim.org page][TagList]. Or if you [use pathogen with git submodules][27] then you might want to use the [git mirror of taglist.vim][tg] instead.

### Customizations ###

The exuberant tags program supports over [40 programming languages][ctl], but it doesn’t know anything about PML. Luckily it’s quite easy to extend exuberant tags to support other languages, by adding rules to a `~/.ctags` file. In the repository for this plugin, you'll find a file called [`ctags`][ctagrules]. Append the contents of this file to your `~/.ctags` file. The exuberant tags site has more information on [extending ctags to support other languages][ctext].

The taglist plugin needs to know where you have installed exuberant tags. You can find out by running `which ctags`. On my system, this returns "/usr/local/bin/ctags". Put this line in your vimrc file:

    let Tlist_Ctags_Cmd = "/usr/local/bin/ctags"

You might also want to create a mapping so that you can quickly toggle the taglist table of contents. For example, if you put these lines in your vimrc:

    let mapleader = ","
    nmap <Leader>/ :TlistToggle<CR>

You could now toggle the taglist by pressing `,/` (comma forward-slash).

Fold by section
---------------

The pml.vim plugin includes a custom folding expression, which allows you to fold sections of your PML document. This screenshot shows how a document looks when opened, with all `<sect1>` tags folded. Note that the fold indicates how many lines have been hidden, and shows the title of the folded section.

![All sect1 sections folded](https://github.com/nelstrom/vim-pml/raw/master/screenshots/folding-1.png)

The next screenshot demonstrates how the document looks after opening the first `<sect1>` tag. Note that the `<sect2>` blocks are indented more than the `<sect1>` blocks, and the section level is indicated in brackets:

![One sect1 opened](https://github.com/nelstrom/vim-pml/raw/master/screenshots/folding-2.png)

If you just learn one folding command, make it this one: `zi`. This toggles folding behaviour on and off. You'll want folding disabled until you learn some of the finer grained folding commands. Then run `:help fold-commands` and swat up.

### Folding other elements

By default, the `sidebar` and `figure` elements are also folded. If you want to customize the list of elements that are folded, you can do so by assigning a list of element names to the `g:pml_foldable_elements` global variable. For example, if you included this line in your `vimrc`:

    let g:pml_foldable_elements = ['story','sidebar']

This would make `story` and `sidebar` elements foldable (but not `figure` elements).

### Room for improvement ###

The table of contents (TOC) generated by this method is rough and ready. It's just a list of links to `<title>`s from throughout the document. That means that it includes the title of figures, e.g.:

    <figure id="fig.soft.vs.hard.wrapping">
      <title>Soft versus hard wrapping</title>
      <imagedata width="full" fileref="images/eps/soft-V-hard-wrap.eps"/>
    </figure>

Ideally, I would prefer if the all figures were collected in a separate list. Also, I would like it if the TOC showed the heirarchy of a chapter by indenting `<title>`s from `<sect2>` and `<sect3>`. These optimisations would be nice, but even without them, this is a very useful feature.

Generating a vimball
--------------------

To distribute the script on [vim.org][s] wrap it up as a vimball by following these steps:

* first, ensure that the file `pml.vba` is not loaded in a Vim buffer
* open the file `vimballer` in Vim
* set the variable `g:vimball_home` to the development directory of this plugin (e.g. run: `:let g:vimball_home='~/dotfiles/vim/bundle/pml'`)
* visually select all lines in `vimballer` file
* run `'<,'>MkVimball! pml.vba`

That should create a file called `pml.vba` which you can upload to [vim.org][].

Credits
-------

Written and maintained by Drew Neil, with contributions from:

* Brendan McAdams (created the omni-completion) 
* Nathan Eror (created the snipMate snippets)
* Dion Nicolaas

[Pathogen]: http://www.vim.org/scripts/script.php?script_id=2332
[pg]: http://github.com/tpope/vim-pathogen
[s]: http://www.vim.org/scripts/script.php?script_id=2540
[sg]: https://github.com/vim-scripts/snipmate
[TagList]: http://www.vim.org/scripts/script.php?script_id=273
[tg]: https://github.com/vim-scripts/taglist.vim
[exuberant]: http://ctags.sourceforge.net/
[27]: http://vimcasts.org/e/27
[ctl]: http://ctags.sourceforge.net/languages.html
[ctext]: http://ctags.sourceforge.net/EXTENDING.html
[snips]: https://github.com/nelstrom/vim-pml/blob/master/snippets/pml.snippets
[ctagrules]: https://github.com/nelstrom/vim-pml/blob/master/ctags
[wiki]: http://www.pragprog.com/wikis/authors/VimTools
[vim.org]: http://www.vim.org/scripts/index.php
[RagTag]: https://github.com/tpope/vim-ragtag
