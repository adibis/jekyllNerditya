---
layout: post
title: "A guide to neovim"
category: code
---


###### Last Updated:
```
07-13-2017: Removed neovim defaults, added more keybindings.
10-28-2016: Removed neovim defaults.
```

I've been using vim ever since I shifted to linux a few years ago. Over the years my current vim config has gathered a lot of random settings which I probably no longer need. A few weeks ago I came across the neovim project. Neovim plans to re-write vim making it easier to maintain while providing better plugin structure, UI arcitecture, async-execution to name a few. Anyone acquianted with vim knows the horrible implementation of vim plugins and the horde of plugins to manage other plugins. I believe neovim is a welcome change, much needed to keep the project managable.

After discovering neovim, I decided to write a new rc file for it based on my vim config, the sensible-vim project and a few other resources I found on the topic. This is an overview of my neovim config, how I use it and the rationale behind some of the settings.

First of all, you no longer need to set nocompatible. It's now a default for neovim, and with that here's a few things that work for me.

## Make use of the Leader
Leader key is like a command prefix. You can map actions to ```<Leader>key``` to easily do some complex stuff. For some reason most of the configurations map the comma key to leader. While that may work I decided to map the leader key to space. I don't really have a use for the spacebar in normal mode and, having the most prominent key on the keyboard for easy use with both hands makes a difference.

```vim
" Map the leader key to SPACE
let mapleader="\<SPACE>"
```

With this, I can now set ```<Leader>o``` to open CtrlP or ```<Leader>f``` to maximize current split. It works beautifully.

## Some basics
Here's some of the settings I use which make vim UI and behavior a bit saner. I've tried adding comments to all of them so they make sense.

```vim
    set showmatch           " Show matching brackets.
    set number              " Show the line numbers on the left side.
    set formatoptions+=o    " Continue comment marker in new lines.
    set textwidth=0         " Hard-wrap long lines as you type them.
    set expandtab           " Insert spaces when TAB is pressed.
    set tabstop=4           " Render TABs using this many spaces.
    set shiftwidth=4        " Indentation amount for < and > commands.

    set linespace=0         " Set line-spacing to minimum.
    set nojoinspaces        " Prevents inserting two spaces after punctuation on a join (J)

    " More natural splits
    set splitbelow          " Horizontal split below current.
    set splitright          " Vertical split to right of current.

    if !&scrolloff
        set scrolloff=3       " Show next 3 lines while scrolling.
    endif
    if !&sidescrolloff
        set sidescrolloff=5   " Show next 5 columns while side-scrolling.
    endif
    set nostartofline       " Do not jump to first character with page commands.
```

I also like to always use spaces instead of tabs and don't like any trailing whitespace in my files. Following code will highlight all tabs and trailing whitespaces in the file.

```vim
" Tell Vim which characters to show for expanded TABs,
" trailing whitespace, and end-of-lines. VERY useful!
if &listchars ==# 'eol:$'
  set listchars=tab:>\ ,trail:-,extends:>,precedes:<,nbsp:+
endif
set list                " Show problematic characters.

" Also highlight all tabs and trailing whitespace characters.
highlight ExtraWhitespace ctermbg=darkgreen guibg=darkgreen
match ExtraWhitespace /\s\+$\|\t/
```

While searching I prefer to have smart-case. Search will match either case unless at least one letter in search pattern is capital. You would also notice annoying highlights from previous searches sometimes, the ```<C-L>``` mapping gets rid of that. I also rend to do search and replace quite a lot and tying the whole ```%s/<search>/<replace>/``` gets too slow. So I added a binding ```<Leader>s``` to put the whole command and put the cursor at the search part of it.

```vim
set ignorecase          " Make searching case insensitive
set smartcase           " ... unless the query has capital letters.
set gdefault            " Use 'g' flag by default with :s/foo/bar/.
set magic               " Use 'magic' patterns (extended regular expressions).

" Use <C-L> to clear the highlighting of :set hlsearch.
if maparg('<C-L>', 'n') ==# ''
  nnoremap <silent> <C-L> :nohlsearch<CR><C-L>
endif

" Search and Replace
nmap <Leader>s :%s//g<Left><Left>
```

Another really useful feature of vim is relative numbers. When turned on, the line numbers will start counting up from current line on either side. Extremely useful when you want to yank a few lines starting from the current line.

```vim
" Relative numbering
function! NumberToggle()
  if(&relativenumber == 1)
    set nornu
    set number
  else
    set rnu
  endif
endfunc

" Toggle between normal and relative numbering.
nnoremap <leader>r :call NumberToggle()<cr>
```

You can also call this function every time you enter or exit the insert mode if you like that.

## Shortcuts
This is one thing that can never be said enough. Get rid of unnecessary keystrokes. Almost every post on this topic will mention this tip yet it amazes me how many people don't take advantage of this. Some of the mappings that I've heard from others and found really useful are included here.

```vim
nnoremap ; :    " Use ; for commands.
nnoremap Q @q   " Use Q to execute default register.
```

## Plugins
Even with the plugin structure improved in neovim, I still think a plugin manager makes it a breeze to install and manage plugins easily. Especially if you're tracking your nvimrc with git (which I strongly recommend you do). I am using vim-plug as my plugin manager. The best thing about vim-plug is its ability to download plugins in parallel using the neovim job control. You may not notice the difference if you're only using 4-5 plugins like I am, but I've seen people use over 40 and you will definitely notice the difference.

![vim-plug](https://raw.githubusercontent.com/junegunn/i/master/vim-plug/installer.gif)

I've included vim-plug in the autoload directory in my configuration as was the intention of the developer. Other plugin managers such as vundle will also update themselves by adding itself in the plugin list. For vim-plug, you need to manually download it from the git repository. (In future I might add a script to update vim-plug).

###1. CtrlP
This is by far the most used plugin I have. CtrlP is a fuzzy finder for vim. It's written in pure vimscript eliminating most of the weird dependencies you have with such plugins. CtrlP can search for files in either the current directory, or the whole project in case of a version control system such as Git. You can also use it to search between currently open buffers, which is the most useful thing for me. I don't like to use tabs much and I don't even like to have multiple splits open and consume screen space a lot of times. I just close all the other buffers and use CtrlP to switch between them. I have then mapped the following in my rc file for easy execution.

```vim
" Open file menu
nnoremap <Leader>o :CtrlP<CR>
" Open buffer menu
nnoremap <Leader>b :CtrlPBuffer<CR>
" Open most recently used files
nnoremap <Leader>f :CtrlPMRUFiles<CR>
```

It can do a lot of other things right from ignoring certain files while searching, maximum cache size, config files to set custom project root for projects not in Git (boo). You can get more information about CtrlP [here](http://kien.github.io/ctrlp.vim/).

###2. Airline
Airline is a statusline and tabline plugin. It's really useful when you work with a lot of buffers (like you should to take advantage of neovim). It displays information about the file, your current mode, position in the file, row/column and you can customize most of its aspects. Really useful is when you have multiple tabs open with each of them having multiple buffers - airline will show how many buffers are open per tab. You can use tabs like they were meant to be; as layouts rather than a tab as in a browser.

![airline](https://github.com/bling/vim-airline/wiki/screenshots/demo.gif)

My settings for airline are:

```vim
let g:airline#extensions#tabline#enabled = 2
let g:airline#extensions#tabline#fnamemod = ':t'
let g:airline#extensions#tabline#left_sep = ' '
let g:airline#extensions#tabline#left_alt_sep = '|'
let g:airline#extensions#tabline#right_sep = ' '
let g:airline#extensions#tabline#right_alt_sep = '|'
let g:airline_left_sep = ' '
let g:airline_left_alt_sep = '|'
let g:airline_right_sep = ' '
let g:airline_right_alt_sep = '|'
let g:airline_theme= 'gruvbox'
```

##Conclusion
I've been fine-tuning the settings every week or so. This is what works for me and I am happy with it. I can sense that I am more confident with navigating multiple files and large projects even without using a file browser plugin or syntax helpers. That might change later on but for now, I like the minimalist approach. Don't agree? [Fork it](http://github.com/adibis/.nvim) and modify to your linking.
