+++
title = "histeq"
description = """This is a description"""
date = "2013-10-13"
draft = true
+++

I recently learned about what is known in signal processing as histogram
equalization.The basic idea is that you will map each value of a signal (in
image processing typically pixel intensities), through the cumulative
probability distribution function for the signal. The end result is that you
will increase contrast by making the histogram of the signal be more uniform.
I've applied this concept to my heightmap generation algorithm with some good
results.

Here is what happens if you take the same terrain types I previously used,  and
spread them out uniformly over the heightmap:

There isn't enough water or mountain regions, and the grassland is too
spacious. The reason is that these grassy areas are in the middle of the
spectrum of terrain types, but these values are over-represented due to the law
of large numbers, and in a small part due to the smoothing. Here is a histogram
showing what I mean:


Notice how most of the mass is in the center, right where the grassy tiles
live. What we want to see is a more uniform distribution. This will increase
contrast on the map, and give us more ocean tiles, more mountain tiles, and
fewer grass tiles. So we apply histogram equalization and we start to get maps
like this:

These maps are much nicer. Before to get maps like this, I would have had to
manually adjust the weights, with fine grained increments in the range [.4,
.6], and larger jumps near the boundaries. Now I can just stack the terrain
types up, and it magically works. Here is what the equalized histograms tend
to look like:

It isn't quite uniform, and indeed will never be except in a few corner
cases, but the contrast has clearly been enhanced. The end result is that we
no longer see the over representation of grassy areas, and we can see ocean
and mountain areas. The coolest part is that it all happens automatically,
and I no longer have to manually tweak the terrain height mappings.
