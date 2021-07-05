---
layout: post
title: Minimal vim folds
date: 2021-07-05 15:00 -0500
---

I use folds to **reduce** visual information on screen, but Vim's default folds are pretty cluttered. Here's Vim with defaults folds:

![screenshot of vim with default folds](/assets/img/default-folds.png)

I searched the internet to find a good pre-made config, but couldn't find any. So I figured out how to customize it myself. Here's the final result:

![screenshot of vim with minimal visual indicators for folds](/assets/img/fold text.png)

And here's the config:

{% highlight vim linenos %}
" Custom Fold Text
  function! MyFoldText()
      let line = getline(v:foldstart)
      let foldedlinecount = v:foldend - v:foldstart + 1
      return '  '. foldedlinecount . line
  endfunction
  set foldtext=MyFoldText()
  set fillchars=fold:\  
{% endhighlight %}

First is an icon, which indicates a fold. Then a number, the number of hidden lines. Then the first line in the fold. 

For the icon (on line 5), I have a Nerd Font glyph ([search for 'fold'](https://www.nerdfonts.com/cheat-sheet)), which doesn't render on the browser, but works in Vim since I'm using JetBrainsMono Nerd Font. If you don't have a Nerd Font installed, feel free to replace it with whatever character represents a fold to you. 

Also note that on line 8, there is a space after the backslash.

## Tweak for Nord Colorscheme 

One more thing - I'm using the Nord color scheme, and I don't like its default colors for folds. I changed it like so:

```vim
" Nord Overrides
  augroup nord-theme-overrides
    autocmd!
    autocmd ColorScheme nord highlight Folded guibg=#313745 guifg=#556076
  augroup END
```
