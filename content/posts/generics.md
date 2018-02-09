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
only to be immediately, abrasively and arrogantly dismissed by an "experienced"
gopher who is tired of seeing yet another mention of the G-word. This part of
gopher culture is my least favorite part of the community.

In some sense, I get it - adding generics is a scary thing for many veteran
gophers who don't feel the need for generics (of course this doesn't justify
the caustic attitude towards discussion). Done wrong, the beautiful simplicity
of Go could be ruined. Done wrong, generics could also slow the compiler, which
would undermine a core design philosophy of the language. That said, those who
do feel the need for generics (as I do) have long wanted to be able to have a
productive discussion around the topic.

Happily, we may have a chance to at least have a discussion now that
there has been a [call for "experience
reports"](https://github.com/golang/go/wiki/ExperienceReports) from the Go
team. This post constitutes my experience report in favor of eventually adding
some form of generics to Go.

## What I wanted to do

One of my side projects is a
[roguelike](http://www.roguebasin.com/index.php?title=What_a_roguelike_is)
library for go called [gorl](https://github.com/jefflund/gorl). A features I
wanted to add to the library is known as a delta clock. I describe it in more
detail in a [previous
post]({{< ref "deltaclock.md" >}}), but basically the delta clock is a clever
data structure that allows you to efficiently schedule events.
It is basically queue implemented as a linked list except that each node also
stores the amount of time, i.e., a delta, until the next node in the list.  I
have found this sort of scheduler to be very efficient and effective for
scheduling the various interactions in a roguelike game.

The thing I put into the delta clock is a type I call `Entity`. It is just a very
simple one method interface:
```go
type Entity interface {
	Handle(Event)
}
```
Note that `Event` is just an alias for the empty interface used for the sake of
readability. The `Entity` type is pretty much anything and everything in the
roguelike world. This is basically one piece of my take on the Entity-Component
design paradigm for games. If you're familiar with roguelikes, the `Entity`
represents obvious things like monsters and items. However, it also includes
abstract things like timers, triggers and other effects.

It turns out that with all these different concrete `Entity` types running
around, I can actually end up having a few different delta clocks for various
things, e.g., one delta clock for monsters, another for effect timers and so
forth. The basic API of my `DeltaClock` type includes a few core operations:

* Schedule - places an Entity into the clock at a specific time offset
* Unschedule - removes an Entity from the clock
* Advance - moves the delta clock forward in time, returning zero or more
  `Entity` that were scheduled to be processed at the current time tick.

What I wanted to do was to be able to pull `Entity` off various `DeltaClock`
and execute game logic on all the different `Entity` types according to the
`DeltaClock` scheduling.

## What I actually did

The issue with this approach is that `Entity` represent a very diverse set of
types despite having the very simple interface which handles messages of any
type.

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
