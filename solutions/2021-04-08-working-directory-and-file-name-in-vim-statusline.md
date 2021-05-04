---
title: Working directory and file name in vim statusline
permalink: working-directory-and-file-name-in-vim-statusline.html
---

I want my status line to look this specific way. If I open vim in `~/Users/pooria/repos/myproject` then I want my status line to read `~/repos/myproject`.

And then once I have vim open, if I go to edit `~/Users/pooria/repos/myproject/src/index.html` I want my status line to look like: `~/repos/myproject/src/index.html`

Not much to ask for right? It took me ages to figure it out. Apparently no one else wants this. But I'm making this solution page for you anyway, my kindred spirit.

I got it working by creating this vim script function in my .vimrc

```
function! Filename()
  let cwd = substitute(getcwd(),$HOME,'~','')
  let path = expand('%f')
  return cwd . '/' . path
endfunction
```

You can then use that function to put the right path in your status line.

I'm using lightline, so it actually looks like this in my setup:

```
let g:lightline = {
      \'active': {
      \   'left': [ [ 'mode', 'paste' ],
      \             [ 'readonly', 'filename', 'modified' ] ]
      \ },
      \ 'component_function': {
      \   'filename': 'LightlineFilename'
      \ }
      \ }

function! LightlineFilename()
  let cwd = substitute(getcwd(),$HOME,'~','')
  let path = expand('%f')
  return cwd . '/' . path
endfunction
```

But you don't have to use lightline to get it to work, just use that Filename function with the basic statusline, or whatever statusline plugin you're using. 
