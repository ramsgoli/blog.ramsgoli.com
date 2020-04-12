---
title: "coc.nvim: Split If Not Open on jumpDefinition"
date: 2020-04-11T17:17:08-07:00
draft: false
toc: false
images:
tags: 
  - neovim
---

If you're like me, you've recently started using the [coc.nvim](https://github.com/neoclide/coc.nvim) engine for neovim
to give your text editor the magical powers of intellisense. Finally, your hipster friends
who use VSCode have stopped making fun of you for still using a "dumb" text editor! All is well again.
***
One of my favorite features in coc is the ability to use the `gd` and `gr` shortcuts
to jump to either symbol definitions or symbol references. This feature works beautifully
for me if the definition or reference exists in the same file. My cursor magically moves
to the correct location. But, if the target location is in a different file, coc will open the file in a new
buffer and replace my current window with the new buffer. Since the buffer where i
navigated from is still open, I could use `<C-o>` and `<C-i>` to navigate quickly between
the two. However, I more often need to look at the two buffers side by side at the same time
to compare.\
\
Coc gives you the ability to specify different jump commands by setting `"coc.preferences.jumpCommand"`.
Yet none of the available jump commands in vim give you the functionality I just described. If you set
`"coc.preferences.jumpCommand": "vsplit"`, coc will always create a new window split, even if the symbol
you are jumping to is in the same window!
***
Here's a simple function you can define to allow coc to open a vertical split if the symbol
you are jumping to is in a different file, but only move your cursor in the same window if
the symbol exists in the same file. Paste this in your `init.vim`
```
function! SplitIfNotOpen(call, fname)
    let bufnum=bufnr(expand(a:fname))
    let winnum=bufwinnr(bufnum)
    if winnum != -1
	" Jump to existing split
	exe winnum . "wincmd w"
    else
	" Make new split as usual
	exe "vsplit " . a:fname
    endif
    " Execute the cursor movement command
    exe a:call
endfunction

command! -nargs=+ CocSplitIfNotOpen :call SplitIfNotOpen(<f-args>)
```
And set `"coc.preferences.jumpCommand": "CocSplitIfNotOpen"` in your `coc-settings.json` file 
(You can open this file by executing the `:CocConfig` command).
***
Voila! Coc will now open a vertical split if jumping to a new file, but simply move the
cursor to the correct position if jumping to the same file.
