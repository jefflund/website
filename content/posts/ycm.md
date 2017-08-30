+++
title = "Vim Plugin of the Day: YouCompleteMe"
description = """YouCompleteMe is easily the best auto-complete engine for \
Vim in existence. With this plugin, Vim becomes a full-powered IDE."""
date = "2013-07-19"
banner = "img/valloric.jpg"
+++

Before I get started, you can find the code and documentation for YouCompleteMe
at: https://github.com/Valloric/YouCompleteMe

I have long searched for a good auto-complete for Vim. I've experimented with
both RopeVim and JediVim and been disappointed for various reasons. I've just
started using YouCompleteMe and so far it has been awesome. The fact that it
works across so many languages (including Python, C++ and Go) and that it is
extremely fast compared to other auto-completers makes this an excellent plugin
in my book.

From what I understand, YouCompleteMe started as a 20% project by Val Markovic
at Google. The in-house version (which I've used) includes all sorts of
Google-specific stuff, but the base project is very usable outside Google.

It took a bit of work to get installed and configured, but it really wasn't too
bad. Just follow the instructions in the Full Installation Guide (changing the
apt-get commands the equivalent yum commands if you use Fedora). Unlike most
plugins, you have to compile so C++ code to get the YCM server working.
However, the server-plugin architecture is nice for two reasons: first, it
makes the auto-complete server much faster as it is written in C++ instead of
Vimscript (or Python inside of Vim). Second, it decouples Vim from the
auto-completer so if something goes wrong or takes a while (it rarely does
btw), then Vim continues to run just fine.

Underneath the covers, the YCM server farms out its work to other completers.
Having a common interface for Vim to access all these completers is quite nice.
It also makes it (relatively) easy to add new completers. Out of the box, YCM
works with C/C++ using Clang, C# using OmniSharp, Python using Jedi, and Go
using Gocode.

Probably my favorite feature of YCM though is the fact that filtering is done
on subsequence matching, rather than prefix matching. For example, suppose I
have a bunch of types all named `<Something>Event`, and I can't remember
exactly which one I'm looking for. I can just type `Evt`, and the pretty much
every Event type will come up. I can be really lazy, or remember just a part of
the name I'm looking for, and then tab complete the rest. Since YCM is gives a
common facade for all the underlying completers, this works regardless of which
language you are working in.

As of July 2013, on Fedora at least, make sure you have run yum update to get
the latest version of patches for Vim as YCM does take advantage of a few
recent bug fixes for Vim. Also, I found it easier to use libclang from the
system, as mentioned in the docs.

Finally, I did some light configuration of the plugin as follows:
```
let g:ycm_autoclose_preview_window_after_insertion=1
let g:ycm_key_list_select_completion=[]
let g:ycm_key_list_previous_completion=[]
```
The first option makes the preview window go away after I leave insertion mode.
I find it is handy while I type, but I want it gone once I am done typing.
There are other options to remove it as soon as you finish the completion, or
to not show the preview at all.

The last two basically tell YCM to leave my key bindings alone. In particular,
it overrode snipMate bindings based around tab, which I found annoying.
However, YCM uses Vim's popup menus so you can navigate the auto-completes
easily with the usual <c-n> and <c-p>, which is what I am used to
anyways.
