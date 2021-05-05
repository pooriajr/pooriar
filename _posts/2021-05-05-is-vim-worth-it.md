---
layout: post
title: Is vim worth it?
date: 2021-05-05 10:12 -0500
---

TL;DR No, not really.

I like vim. I use vim. I have customized the pants off of vim. There's nothing on this earth, short of my own skin, that is more molded to me, Pooria Rashidi, than my vim config.

And that's the problem.

## Customization

If you want to use vim without customizing it, think again.

If you've used a modern text editor like VSCode, Vim's defaults are gonna feel atrocious. 

Right away you'll notice there's no syntax highlighting, no line numbers. You turn those options on, and the the colors are appalling. So you find a good color scheme, but it doesn't work right out the box. You have to start fiddling with a few other settings and - oh no! You're customizing vim! Stop!

Honestly, I tried for a while. I really tried. I kept my customizations very minimal. "Just a few standard settings, no plugins".

But the siren song of frictionless writing was too strong. I wanted to blur the division between my thoughts and the text that flowed into the computer. I installed a few essential plugins, and it was amazing. Whenever a new bit of friction showed up in my process I could solve it with a plugin that some nerd saint had made a decade ago.

This was salvation. Typing nirvana. I went all in.

## One year later

Below is my vimrc at the time of writing. I just cleaned it up recently by the way. So it's shorter than usual.

Don't read it (please), just scroll by in amazement at how someone could spend so much time configuring a text editor.

{% highlight vim lineno %}
set smartcase
set ignorecase
set cursorline
set hidden
set mouse=a
set tabstop=2
set shiftwidth=2
set expandtab
set smartindent
set shiftround 

set undofile
set undodir=$HOME/.vim/undo

syntax on

" Leader
let mapleader = " "

" Change cursor by mode in Iterm2
let &t_SI = "\<Esc>]50;CursorShape=1\x7"
let &t_SR = "\<Esc>]50;CursorShape=2\x7"
let &t_EI = "\<Esc>]50;CursorShape=0\x7"

" Improve splits
set splitbelow
set splitright
nnoremap <C-h> <C-W>h
nnoremap <C-j> <C-W>j
nnoremap <C-k> <C-W>k
nnoremap <C-l> <C-W>l

" Shortcuts for standard actions
nnoremap <leader>q <C-W><C-Q>
nnoremap <leader>w :w<CR>
nnoremap j gj
nnoremap k gk
nnoremap <leader>/ :nohl<CR>
inoremap <C-d> <Del>
vnoremap <leader>c "*y
nnoremap !! :!!<CR>

" Meta Shortcuts
nnoremap <leader>vv :tabedit ~/.vimrc<CR>
nnoremap <leader>vr :source $MYVIMRC<CR> 
nnoremap <leader>vp :PlugInstall<CR>

" Terminal
nnoremap <leader>t :terminal<CR>
tnoremap <Esc><Esc> <C-\><C-n>
tnoremap <C-j> <C-W>j
tnoremap <C-k> <C-W>k
tnoremap <C-l> <C-W>l

" Auto install vim plug if it doesn't already exist
if empty(glob('~/.vim/autoload/plug.vim'))
  silent !curl -fLo ~/.vim/autoload/plug.vim --create-dirs
        \ https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
  autocmd VimEnter * PlugInstall --sync | source $MYVIMRC
endif

" Specify a directory for plugins
call plug#begin('~/.vim/plugged')

" Navigation
Plug 'junegunn/fzf', { 'do': { -> fzf#install() } }
Plug 'junegunn/fzf.vim'
Plug 'tweekmonster/fzf-filemru'
nnoremap <leader>r :Rg<CR> 
nnoremap <leader>f :FilesMru --tiebreak=end<CR>

" NerdTree
Plug 'preservim/nerdtree'
nnoremap <leader>nn :NERDTreeToggle<CR>
nnoremap <leader>nf :NERDTreeFind<CR>

" Easymotion
Plug 'easymotion/vim-easymotion'
let g:EasyMotion_do_mapping = 0 " Disable default mappings
" Jump to anywhere you want with minimal keystrokes, with just one key binding.
nmap s <Plug>(easymotion-overwin-f2)
" Turn on case-insensitive feature
let g:EasyMotion_smartcase = 1
" JK motions: Line motions
map <Leader>j <Plug>(easymotion-j)
map <Leader>k <Plug>(easymotion-k)
map <Leader>l <Plug>(easymotion-lineforward)

" Formatting
Plug 'tpope/vim-surround'
let g:surround_{char2nr('=')} = "<%= \r %>"
let g:surround_{char2nr('-')} = "<% \r %>"

Plug 'tpope/vim-commentary'
Plug 'tpope/vim-endwise'
Plug 'tpope/vim-repeat'
Plug 'tpope/vim-abolish'
Plug 'tpope/vim-unimpaired'
Plug 'mattn/emmet-vim'
Plug 'AndrewRadev/splitjoin.vim'

Plug 'AndrewRadev/sideways.vim'
nnoremap H :SidewaysLeft<cr>
nnoremap L :SidewaysRight<cr>
omap aa <Plug>SidewaysArgumentTextobjA
xmap aa <Plug>SidewaysArgumentTextobjA
omap ia <Plug>SidewaysArgumentTextobjI
xmap ia <Plug>SidewaysArgumentTextobjI
nmap <leader>si <Plug>SidewaysArgumentInsertBefore
nmap <leader>sa <Plug>SidewaysArgumentAppendAfter
nmap <leader>sI <Plug>SidewaysArgumentInsertFirst
nmap <leader>sA <Plug>SidewaysArgumentAppendLast

" Rails
Plug 'tpope/vim-rails'
Plug 'tpope/vim-dispatch'
Plug 'tpope/vim-rbenv'

" Git
Plug 'tpope/vim-fugitive'
nnoremap <leader>g :Git 

Plug 'airblade/vim-gitgutter'
set updatetime=500 " update gitgutter every 500 milisecond
noremap <leader>hh :GitGutterToggle<CR>

" Aesthetics
set termguicolors
Plug 'cocopon/iceberg.vim'

Plug 'Konfekt/FastFold'
Plug 'nathanaelkane/vim-indent-guides'
let g:indent_guides_enable_on_vim_startup = 1
let g:indent_guides_auto_colors = 0
autocmd VimEnter,Colorscheme * :hi IndentGuidesOdd  guibg=#161821   ctermbg=3
autocmd VimEnter,Colorscheme * :hi IndentGuidesEven guibg=#1E2131 ctermbg=4

" Lightline
Plug 'itchyny/lightline.vim'
" hide the default status line since we have lightline
set noshowmode 
let g:lightline = {
      \ 'colorscheme': 'iceberg',
      \'active': {
        \   'left': [ [ 'mode', 'paste' ],
        \             [ 'gitbranch', 'readonly', 'filename', 'modified' ] ]
        \ },
        \ 'component_function': {
          \   'filename': 'LightlineFilename',
          \   'gitbranch': 'FugitiveHead'
          \ }
          \ }

function! LightlineFilename()
  let cwd = substitute(getcwd(),$HOME,'~','')
  let path = expand('%f')
  return cwd . '/' . path
endfunction

" Language Support
Plug 'sheerun/vim-polyglot'
Plug 'tpope/vim-liquid'
Plug 'evanleck/vim-svelte', {'branch': 'main'}

" Text objects
Plug 'kana/vim-textobj-user'
" e entire file
Plug 'kana/vim-textobj-entire'
" r ruby block
Plug 'nelstrom/vim-textobj-rubyblock'
" e erb tag
Plug 'whatyouhide/vim-textobj-erb' 
" i indent level
Plug 'michaeljsmith/vim-indent-object'

" UNIX
Plug 'tpope/vim-eunuch'

" GOYO
Plug 'junegunn/goyo.vim'
nnoremap <leader>G :Goyo<CR>

function! s:goyo_enter()
  let b:quitting = 0
  let b:quitting_bang = 0
  autocmd QuitPre <buffer> let b:quitting = 1
  cabbrev <buffer> q! let b:quitting_bang = 1 <bar> q!
  set linebreak
endfunction

function! s:goyo_leave()
  " Quit Vim if this is the only remaining buffer
  set nolinebreak
  if b:quitting && len(filter(range(1, bufnr('$')), 'buflisted(v:val)')) == 
    if b:quitting_bang
      qa!
    else
      qa
    endif
  endif
endfunction

autocmd! User GoyoEnter call <SID>goyo_enter()
autocmd! User GoyoLeave call <SID>goyo_leave()

" Settings
Plug 'tpope/vim-sensible'

" Other
Plug 'mhinz/vim-startify'
Plug 'vim-scripts/restore_view.vim'
Plug 'ackyshake/VimCompletesMe'
Plug 'rstacruz/vim-xtract'
Plug 'psliwka/vim-smoothie'

" Initialize plugin system
call plug#end()

" this has to be down here or it doesnt work with vim-plug 🤷‍♂️
colorscheme iceberg
let macvim_skip_colorscheme = 1
set background=dark

" That's all, folks!

{% endhighlight %}

## Hours, then Days

As time went on, I got more and more finicky with my expectations. I wanted every speck of frustration wiped out of my text editing process.

I've always been obsessive about this type of thing, but vim spoiled me. It showed me how *perfect* text editing has the potential to be - if you only just train the computer well enough. 

So I spent more and more time training vim into my *perfect* editor.

At this point, I have easily sunk over 24 hours into tweaking and configuring vim. That's not including the time it took to learn it in the first place, or the amount of time I've spent **thinking** about it, which is definitely not negligible. It's a weird form of procrastination, because it seems to promise productivity returns that far outweigh the initial investment.

I don't really know how much productivity it has actually bought me. Surely *some* at least. What I do know is that it has cost me productivity many times when a tiny nuisance or random idea derailed me from my work and sent me off on a wild goose chase for a new plugin.

## Typing Obsession

You might be thinking, "Damn, this guy sounds like a drug addict with a text editor." 

I'll admit it seems strange to be so **involved** with the way I type. But considering that a) I have nerdy tendencies and b) typing is the direct channel through which a lot of my creativity and livelihood flow through, I don't think my concern is totally ridiculous.

Oh, but anyway, let's get back to why you shouldn't learn vim.

## Don't do it

Vim is elegant. It will turn the physical act of typing into a dance of the fingers, but it won't make your output any better.

Vim is fast. It will speed you up in 1000 minor ways, but slow you down massively in a few major ways.

Vim is fun. It will make you enjoy typing more as you refine it for your unique needs. And in the process, it will distract you from what really matters.

Vim is powerful. There's truly no end to what it can do, but that endless possibility will keep you always searching for me.

Vim is addicting. Once you use it, it's hard to go back to the old way of typing. And even if you do, you'll never forget how much power you've lost.

Don't learn vim. 

Focus on what you type, not how you type.

Thanks for reading!

.

.

.

.

But... 

That being said...

If you do want to learn vim... 

Well... who's going to stop you?

`:wq!`
