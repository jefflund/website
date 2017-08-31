+++
title = "The G-word in Go"
description = """This is my experience report in favor of adding generics to Go 2.0"""
date = "2017-08-29"
banner = "/img/gopher.png"
+++

# Experience Report

For much of the Go community, the word 'generics' is something of a naughty
word. There are countless posts on the
[go-nuts](https://groups.google.com/forum/#!forum/golang-nuts) discussion list
where someone asks an innocent or reasonable question involving the G-word,
only to be immediately and abrasively shot down by an experienced gopher who is
tired of seeing discussion around generics.

In some sense, I get it - adding generics is a scary thing for many veteran
gophers who don't feel the need for generics. Done wrong, the beautiful
simplicity of Go could be ruined. Done wrong, generics could also slow the
compiler, which would undermine a core design philosophy of the language.
That said, those who do feel the need for generics as I do have long wanted to
be able to have a productive discussion around the topic.

Happily, we may have a chance to at least have a discussion now that
there has been a [call for "experience
reports"](https://github.com/golang/go/wiki/ExperienceReports) from the Go
team. This post constitutes my experience report in favor of eventually adding
some form of generics to Go.

## What I wanted to do

DeltaClock with Actor

Actor is a simple one method interface

Actor implementations may be more complicated, and may need implementation
details beyond Act

## What I actually did

Type assertions every where!

## Why this wasn't great

Type assertions make code brittle, complicated, and unreadable

# Thoughts on a solution

I know the Go team doesn't want solutions (yet). Nevertheless, this is my blog
so I'll put a few thoughts out there for when (hopefully) the Go team decides
that we need some form of generics.

## Generic data structures not algorithms

Interfaces already let you do generic algorithms (see sort package)

Cannot cleanly do generic data structures (see container package)

In the name of simplicity, don't add generic algorithms. Don't need to be able
to write generic functional style map/fold/filter when a loop does that. Don't
need to be able to write generic function like sort when interfaces already
lets you do that.

## Implementation - we've already got some examples

Since we're only talking containers, can do what slices and maps do! Only need
to know the size of the thing the container holds, nothing else fancy.

Don't do erasure. Do what slices and maps do.

## Syntactic considerations

Let us use make/append/delete as needed.

Use [] syntax as with maps, channels, and slices
