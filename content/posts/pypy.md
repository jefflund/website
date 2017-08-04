+++
title = "PyPy Performance Gains"
description = """Sometimes you really can get something for nothing. Here are \
my performance gains simply by switching to pypy."""
date = "2012-03-31"
draft = true
+++

https://us.pycon.org/2012/schedule/presentation/243/

After watching this talk from PyCon 2012 about optimizing for PyPy, I did some
simplification in my python topic modelling code base. Essentially all I did
was replace some objects which wrapped dicts with a bunch lists of int. It is
slightly less memory efficient this way, since these dicts where fairly sparse,
but my python code base now has a runtime on par with my Java code. Sa-weet!

Here are some LDA run times on a super tiny test set:
python with counters: 64.1591711044
python with lists: 19.5545578003
pypy with counters: 3.84229207039
pypy with lists: 1.64124798775

I'll more realistic numbers for 20 newsgroups or something later on...
