+++
title = "Mixing C into Python"
description = """I wish I had known how incredibly easy ctypes is. \
Here I show how rewriting the inner loop of my Python code in C led to \
impressive performance gains."""
date = "2012-09-15"
draft = true
+++

http://www.jiaaro.com/python-performance-the-easyish-way

I wish I had known how incredibly easy ctypes is in Python.

Developing in Python is a dream. Even if Python were 10x slower than it is now
I think I would still choose Python because it is the best language out there
for quickly turning my ideas on a whiteboard into working code running on the
supercomputer. The alternatives like Java or C are just not appealing, and add
painful hours to development, testing, and deployment.

However, sometimes you just need a bit more oomph from Python than it can give.
Of course PyPy helps a lot (I previously shared a case where PyPy outperforms
my old Java code). In this case it improved speed by an order of magnitude, but
Java still creamed us.

Obviously, the first step is to identify bottlenecks and write better code
inside those inner loops. Rewriting this inner loop with a library call instead
of our own code yield significant improvements. We were getting closer to Java
using PyPy. Still, we wanted more!

We followed the tutorial in the link, and rewrote the inner loop as a single C
function. Writing a huge and complicated system in C is gross, but writing a
single number crunching function is really quite doable. With ctypes, calling
that function from Python turns out to be really easy!

Here are some numbers:

Naive Python - 1E7 samples in 6:40

Naive PyPy   - 1E8 samples in 3:12

Rewritten PyPy - 1E9 samples in 2:30

Original Java - 1E9 samples in 1:11

Python + ctypes - 1E9 samples in 0:15
