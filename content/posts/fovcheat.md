+++
title = "fovcheat"
description = """This is a description"""
date = "2013-02-05"
draft = true
+++

If you think about it, just about every field of view algorithm ends up doing
exactly the same work over and over again, just translated to a new place on
the map. For example, consider the popular shadow casting algorithm. Depending
on what terrain you find, shadow casting ends up computing start and stop
slopes to determine which tiles should be considered for the next recursive
scan. However, the end result is always the same -  the visibility for one tile
depends entirely on the visibility of one or two tiles in front of it. The
slope calculations are just a way of figuring out these tile dependencies. If
these calculations can be cached, we can get a quality field of view algorithm
which at run time will be extremely fast.

To this end I got out some (virtual) graph paper and started trying to figure
out what those dependencies should be. For simplicity, I wanted each tile to
have exactly one dependency. Essentially what I started doing was drawing lines
from the source tile out to each tile in one quadrant and seeing which tile
behind it would be blocked. That that became the dependency. The only trick was
finding a pattern so that I could extend the pre-calculating to an arbitrary
radius. This is what I came up with:

Hopefully you can see the pattern. Essentially if you can figure out the red
arrows which have two dependent tiles, the rest falls into place. The number of
times on a particular row that that row maps to two tiles increases by one for
each row. Once you have done this for one quadrant, with just a few rotations
and reflections, you can get the graph for the other seven quadrants. I don't
know if this continues to look good past what I have show here, but for these
smaller distances, it looks just fine. For the purposes of Ruin, this is great.

If you wanted to do this with a different algorithm, I think you still could.
All you do is run your field of view algorithm multiple times, pretending there
is just one tile. You then see which tiles are blocked, and make those tiles
dependent on the blocking tile. Repeat the process out to the depth you care
about, and you have your pre-computed dependency graph. As with my graph, you
only have to do this for one quadrant.

There is one final problem which must be fixed. If you are looking down a
horizontal or vertical corridor, you will only be able to see the walls
immediately surrounding you. This looks pretty weird. You might think that you
could fix this by adding in arrows to the graph, but then you get some really
weird looking ways to peak around corners which are very far away. Instead, I
applied a post-processing step where I can in each orthogonal direction. Until
I hit a wall, I assume that if I can see a tile directly orthogonal to the
origin, I can see the the two tiles which are next too that tile.

Here are some screenshots of the final results:

