---
title: Vim proper formatting for inline CSS and JS in html files
permalink: vim-indent-js-inline-html.html
---

Vim's built in auto-indent doesn't work well on javascript that's inside `<script>` tags in an html file.
It also struggles with css that inline in `<style>` tags in an html file.

The simplest option is to merely split out your styles into .css files and scripts into .js files but sometimes you want to keep it inline.

I tried a bunch of options, and here's the only one that worked:

1. Install this vim plugin: [vim-autoformat](https://github.com/Chiel92/vim-autoformat)
2. Install js-beautify **globally** (via node: `npm install -g js-beautify`)
3. Set up a shortcut for formatting. Mine is `Leader` + `=` (`noremap <leader>= :Autoformat<CR>`) since I'm already used to using `=` for indentation.

Now inline JS and CSS will format properly in your html files. Woo!
