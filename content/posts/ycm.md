+++
title = "Vim Plugin of the Day: YouCompleteMe"
description = """YouCompleteMe is easily the best auto-complete engine for \
Vim in existence. With this plugin, Vim becomes a full-powered IDE."""
date = "2013-07-19"
draft = true
+++

I have long searched for a good auto-complete for Vim. I've experimented with
both RopeVim and JediVim and been disappointed for various reasons. I've just
started using YouCompleteMe and so far it has been awesome. The fact that it
works across so many languages (including Python and C++) and that it is
extremely fast compared to other auto-completers makes this an excellent plugin
in my book.

It took a bit of work to get installed and configured, but it really wasn't too
bad. Just follow the instructions in the Full Installation Guide (changing the
apt-get commands the equivalent yum commands if you use Fedora).

On Fedora at least, make sure you have run yum update to get the latest version
of patches for Vim as YCM does take advantage of a few recent bug fixes for
Vim. Also, I found it easier to use libclang from the system, as mentioned in
the docs.

Finally, I did some light configuration of the plugin as follows:

let g:ycm_autoclose_preview_window_after_insertion=1
let g:ycm_key_list_select_completion=[]
let g:ycm_key_list_previous_completion=[]

The first option makes the preview window go away after I leave insertion mode.
I find it is handy while I type, but I want it gone once I am done typing.
There are other options to remove it as soon as you finish the completion, or
to not show the preview at all.

The last two basically tell YCM to leave my key bindings alone. In particular,
it overrode snipMate bindings based around tab, which I found annoying.
However, YCM uses Vim's popup menus so you can navigate the auto-completes
easily with the usual <c-n> and <c-p>, which is what I am used to
anyways.
