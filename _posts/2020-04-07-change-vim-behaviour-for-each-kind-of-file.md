---
layout: post
title: "Quick vim tip: Change vim's behaviour for each kind of file"
tags: ["vim"]
description: >
  Each kind of file has different code standards, maybe, you would like to set up custom shortcuts for each of them. In the vim editor, you can split how editor should have to behave for each one.
---

We navigate on many different kinds of files every day. If you work in web projects your daily routine probably involves at least four kinds of file, HTML, CSS, Javascript and your language to the backend. 

Each kind of file has different code standards, maybe, you would like to set up custom shortcuts for each of them. In the vim editor, you can split how editor should have to behave for each one.

In the example below I created a file to my Vuejs configs.

```console
$ touch ~/.vim/after/ftplugin/vue.vim
```

And then I wrote the custom configs inside it.

```vim
setlocal shiftwidth=2
setlocal tabstop=2
```

I missed to inform at the start of the post, but, the files inside `ftplugin` directory only will work with the file type detection is on.

```vim
filetype plugin indent on
```

I hope to helped your `.vimrc` to be cleaner.

## More about

* [Keep your vimrc file clean](https://vim.fandom.com/wiki/Keep_your_vimrc_file_clean)
