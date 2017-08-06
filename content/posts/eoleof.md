+++
title = "New Vim Pluging: EOLEOF"
description = """Usually you want a newline at the end of a file. Sometimes \
you don't. Here is how I fixed the problem in Vim."""
date = "2017-02-08"
banner = "img/eoleof.png"
+++

## A bit of history

I wrote a Vim plugin which helps you handle newlines at the end of files (i.e.,
End Of Line End Of File). You can find it on [GitHub]
(http://github.com/jefflund/eoleof). If you look at a bit of POSIX history,
this may seem a bit strange. Normally, you want your text files to end with a
newline. This due to [how POSIX defines a line]
(http://pubs.opengroup.org/onlinepubs/9699919799/basedefs/V1_chap03.html#tag_03_206):
```
3.206 Line
A sequence of zero or more non- <newline> characters plus a terminating <newline> character.
```
Essentially, if the final line of your code or text file doesn't have that
trailing newline character, then the file ends with something other than a
line! In fact it is [defined as an incomplete line]
(http://pubs.opengroup.org/onlinepubs/9699919799/basedefs/V1_chap03.html#tag_03_195):
```
3.195 Incomplete Line
A sequence of one or more non- <newline> characters at the end of the file.
```
For some things, this isn't such a big deal. The biggest annoyance will
be that commands like `cat` behave a little wonky. The difference looks
something like this:
```
$ cat file_without_trailing_newline.txt
abc
def
hij$ cat file_with_trailing_newline.txt
abc
def
hij
$
```
Other times however, missing that trailing newline is a bit more annoying. For
example some compilers actually respect the POSIX standard and may barf up an
warning or error when you ask it to compile a file with an incomplete line.
Considering that most Unix commands and some compilers are built assuming that
files end with proper newline terminated lines, its pretty much always a good
idea to follow this standard. This is why Vim defaults to writing that trailing
newline for you---its nearly always what you want anyways.

## Hugo template troubles

So given all this history, why would you want to *not* have newlines at the end
of files? For me, the answer was [Hugo](http://gohugo.io). Hugo is an excellent
static site generator, and I've been having a lot of fun building this website
using Hugo. However, I ran into a problem getting the output HTML to be nicely
formatted when using various partial templates and shortcodes.

The problem boils down to the fact that partial templates and shortcodes
essentially copy the entire contents of one HTML file and paste them into
another---trailing newlines included. So for example, suppose I was trying to
render the summary for the three most recent posts:
```
<div class="row"
  {{ range first 3 .Data.Pages }}
  <div class="col-sm-4">
    {{ .Render "summary" }}
  </div>
{{ end }}
</div>
```
Supposing that my summary were just a one line `<a>` tag for illustration
purposes, the output HTML would look something like this:
```
<div class="row">

  <div class="col-sm-4">
    <a href="/posts/hello/">hello</a>

  </div>

  <div class="col-sm-4">
    <a href="/posts/hello/">hello</a>

  </div>

  <div class="col-sm-4">
    <a href="/posts/world/">world</a>

  </div>

  <div class="col-sm-4">
    <a href="/posts/foobar/">foobar</a>

  </div>

</div>
```
Notice all the extra newlines that get injected after each shortcode! This is a
pretty simple template, so there is only a few places with extra lines, but for
more complicated templates, you can sometimes end up with half a dozen blank
lines for every non-empty line. Its hard to read, and looks ridiculous. To some
degree you can mitigate the problem using Hugo's whitespace trimming hyphens.
However, no matter what you do with the hyphens, there are some cases where you
can never remove that trailing newline from the summary template without
completely squashing all the output onto a single line.

The solution is simple: remove the trailing whitespace from the summary
template. The problem is not so simple; Vim makes it really hard to do this out
of the box, thus the need for the EOLEOF plugin.

## Vim plugin to the rescue

There are a couple of things EOLEOF does for you to help manage newlines at the
end of files. The first is a statusline flag which lets you see whether or not
the current buffer has a newline at the end of the file. At first I thought you
could just add something like `eol:$` to `listchars`, but it turns out that Vim
is so sure that you want newlines at the end of your files, that it prints an
EOL character at the end of files even when it isn't really there! Adding this
status flag is easy with the following vimrc addition:
```
statusline+=%{EOLEOFStatusline()}
```
The `EOLEOFStatusline()` is actually pretty simple --- all I do is a call to
```
tail -n 1 % | wc --lines`.
```
This works because if the final 'line' is incomplete, then wc will return 0. If
the file ends with a line, then the count is 1.

As for preserving missing newlines at the end of files, I added the `setlocal
nofixeol` to my `ftplugin/html.vim`. With this option, any HTML file without a
trailing newline will not have it automatically added. Unfortunately, I haven't
figured out a clean way to have the EOLEOF plugin cleanly manage this option,
so I just manually put it in my dotfiles as needed.

The final piece of the puzzle is the ability to set, toggle, and unset the
trailing newline. Using
```
truncate --size=-1 %
```
to remove the newline and
```
cat "" >> %
```
to add the newline, EOLEOF also offers the following commands: `EOLEOFAdd`,
`EOLEOFToggle`, and `EOLEOFRemove`. All these commands check to see if the
newline is present, and act accordingly. I also left in the warnings when the
file does change, so that nothing is opaque.
