# Neovim and useful Vim motions

In 1976 Bill Joy created vi, and in 1991 Bram Moolenaar released Vim as a
improved version of vi.

Then in 2014, NeoVim came along. Neovim is higly refactored fork of Vim,
which focuses on extensibility and usability. Neovim is a drop-in replacement
of Vim.

## Neovim vs Vim

### Contributors

[Vim](https://github.com/vim/vim/graphs/contributors)

For Vim, Bram is the one who can decide go or no-go on a feature or something,
he has the total control over the repository. If someone else has the roadmap
or pull-requrest submission that does not align with Bram's thought, and then
it will simply not going to happen.

[Neovim](https://github.com/neovim/neovim/graphs/contributors)

So Neovim first came out because of people are fed up with Bram's leadership,
and his unchallengable authority over the project.

### Features

There are some features that are firstly shipped in Neovim then Vim ended up
adding those features as well.

1. Terminal

    You can use `:terminal` to launch a terminal inside Vim/Neovim. And you may
    find yourself falled into this recursive hell.

    ![img](assets/vim-term.png)

1. Asynchronous action.

    This is more usfully in plugin developemnt, so the codes can be executed in
    the background instead of blocking the current UI frame.

The biggest difference bewteen Neovim and Vim now is Neovim has a built-in
[LSP](https://microsoft.github.io/language-server-protocol/) support, and Lua
support.

1. LSP (Language Server Protocol)

    The provide the API features like auto-complete, go to definition, find all
    references.

1. Lua

    IMO, I think the built-in Lua support is the feature that starts to change
    everything. Some of you may use Vimscript to customize your Vim by editing
    the `.vimrc` file. Vimscript is not capable of being a programming language.
    You may find it really hard to find an answer for a Vimscript question.
    
    Then many great plugins came along for Neovim only, as they are written in Lua.

### Defaults

The [defaults](https://neovim.io/doc/user/vim_diff.html#nvim-defaults) settings of a
brand new Neovim environment is different from Vim.

## Neovim Plugins

You can find Neovim plugins here: [neovimcraft](https://neovimcraft.com/).

We will be focusing on the following plugins:

- [nvim-treesitter](https://github.com/nvim-treesitter/nvim-treesitter)
    The nvim-treesitter will provide you better syntax highlighting, better indention
    support.

- [telescope.nvim](https://github.com/nvim-telescope/telescope.nvim)
    Real cool fuzzy-finder for quick navigation.
    
- [packer.nvim](https://github.com/wbthomason/packer.nvim)
    Plugin manager that can also manage itself, easy to use and apply customizations
    on plugins.

### Neovim Setup

Configure your Neovim:
[0 to LSP: Neovim RC from Scratch](https://www.youtube.com/watch?v=w7i4amO_zaE&t=421s).

My Vim setup:
[vim](https://github.com/pseudocc/dotfiles/tree/vim).

My Neovim setup:
[nvim](https://github.com/pseudocc/dotfiles/tree/nvim).

## Vim motions and tricks

There are a lot of ways you can do certain things in Vim, memorizing everything is not
practical.

1. Basic Vim motions

    - `:h left-right-motions`: h, l, f, t, F, T
    - `:h up-down-motions`: j, k, (), {}, CTRL-U, CTRL-D
    - `:h jump-motions`: CTRL-O, CTRL-I

1. Advanced Vim motions (`:help text-objects`)

    - word: aw, iw, aW, iW
    - sentence/paragraph: as, is, ap, ip
    - blocks: a[, i[, a(, i(, a{, i{, a", i", a', i', a\`, i\`

1. Very useful keybindings

    Move lines up/down, or move character left/right.

    ```lua
    -- move lines up/down or character left/right
    local opts = { silent = true, remap = false }
    local map = vim.keymap
    map.set('n', '<M-k>', [[:m .-2<CR>==]], opts)
    map.set('n', '<M-j>', [[:m .+1<CR>==]], opts)
    map.set('n', '<M-l>', [["9x"9p]], opts)
    map.set('n', '<M-h>', [[h"9x"9ph]], opts)
    map.set('i', '<M-k>', [[<Esc>:m .-2<CR>==gi]], opts)
    map.set('i', '<M-j>', [[<Esc>:m .+1<CR>==gi]], opts)
    map.set('i', '<M-l>', [[<Esc>l"9x"9pi]], opts)
    map.set('i', '<M-h>', [[<Esc>"9x"9phi]], opts)
    map.set('v', '<M-k>', [[:m '<-2<CR>gv=gv]], opts)
    map.set('v', '<M-j>', [[:m '>+1<CR>gv=gv]], opts)
    ```

    Equivalent in Vimscript.
    ```vim
    nnoremap <silent><M-k> :m .-2<CR>==
    nnoremap <silent><M-j> :m .+1<CR>==
    nnoremap <silent><M-l> xp
    nnoremap <silent><M-h> hxph

    inoremap <silent><M-k> <Esc>:m .-2<CR>==gi
    inoremap <silent><M-j> <Esc>:m .+1<CR>==gi
    inoremap <silent><M-l> <Esc>lxpi
    inoremap <silent><M-h> <Esc>xphi

    vnoremap <silent><M-k> :m '<-2<CR>gv=gv
    vnoremap <silent><M-j> :m '>+1<CR>gv=gv
    ```

    Replace virtual mode selection with losing the current
    register.

    ```lua
    local opts = { silent = true, remap = false }
    map.set({ 'n', 'v' }, '<leader>D', [["_d]], opts)
    map.set({ 'n', 'v' }, '<leader>P', [["_dP]], opts)
    ```
