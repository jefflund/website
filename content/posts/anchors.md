+++
title = "Anchor Words Algorithm Overview"
description = """Anchor words is a fast and scalable topic modeling \
algorithm, but the math is a bit dense. Here is a correct but simplified \
probabilistic view of the algorithm."""
date = "2017-08-03"
banner = "/img/anchor.jpg"
+++

# `WARNING: WORK IN PROGRESS`

## Preliminaries

The goal of this post is to give a brief overview of the topic modeling
algorithm known as anchor words. The original series of papers describing the
algorithm are quite mathematically dense, and full of very interesting proofs.
I'll briefly mention these results, but will try and describe the algorithm
from a slightly simpler probabilistic view. We'll start with a brief overview
of what topic modeling is to establish some notation, and then we'll dive into
the anchor algorithm itself. Keep in mind that this post is a bit of a work in
progress so please feel free to send me questions so I can clarify anything.

At this point I should also mention that there is a vast literature describing
various ways of representing and inferring topic models. I'm not to cover any
of them, but I don't want to give the impressing that the algorithm we're
describing here is the only way to do topic modeling.

## Topic Modeling Overview

The goal of topic modeling is to automatically discover topics in large
collections of documents.  For example, if I ran a topic model on a collection
of Amazon product reviews, I might see topics that look something like this:

<figure class="well">
<img src="/img/anchors/topics.png" class="img-responsive"/>
<figcaption>
Caption
</figcaption>
</figure>

I've represented the topics by sets of related words. For example, a topic
concerning various types of wireless routers includes words like `wireless`,
`linksys` `signal`, and `internet`. By looking at the topics discovered by a
topic model, we can get a high level overview of the thematic content of our
data, without having to read it all manually. Its also used by companies like
Google for things like matching ads to search queries.

While we often represent topics as sets of related words, in truth they are
probability distributions over the vocabulary of our data. Once we have these
topic-word distributions, we can answer questions like `p(word|topic)`. Note
that `p(word|topic)` is read as "probability of word given topic". It basically
asks the following: given that I am reading something from a particular topic,
what is the probability of seeing a particular word. For example,
`p("wireless"|router)`, or the probability of seeing the word "wireless" give
that we're talking about routers might be relatively high.
Conversely, `p("lens"|routers)`, or the probability of seeing the word "lens"
while discussing routers might be lower.

So that is basically the goal of topic modeling: learn `p(word|topic)`. In
fact, rather than choosing any arbitrary topic-word distribution, we want to
learn the topic-word distributions which best explains the observed data.
Without going into the details, in general finding the topic-word distribution
which best fits the data is an NP-hard problem. Instead, we need to make some
simplifying assumptions in our model to infer approximate answers.

## Applying Bayes' Rule

In order to see where the simplifying assumptions of the anchor algorithm lie,
we first need to break up our problem a bit using Bayes' Rule. Remember our
goal is to compute `p(word|topic)`. If we apply Bayes' Rule, we'll see that
this quantity is proportional to `p(word) * p(topic|word)`.

The first part of this equation `p(word)` is our prior probability over words.
For example, quantities like `p("quality")`, `p("battery")`, or `p("price")`
might be very common in our Amazon review data, as they are words used
throughout the entire collection of documents. On the other hand, probabilities
like `p("floccinaucinihilipilification")` would be very low, as words like
"floccinaucinihilipilification" are very rarely used in Amazon reviews (if
ever).

Computing these probabilities empirically is actually quite easy. We simply
count up the number of times a particular word occurs in our data, and divide
by the total number of words. Note that these computations do not involve
topics at all. We refer to it as a prior because it is what we assume about the
words prior to looking at the topics.

The second part of this equation `p(topic|word)` is a bit trickier to compute.
It is the inverse conditioning of what we are after. It asks the question of
how likely a topic is given that we just saw a particular word. For example,
quantities like `p(cameras|"lens")` might be very high, since we typically only
use words like "lens" when discussing cameras. On the other hand,
`p(routers|"lens")` should be low, since we aren't typically discussing routers
when we use the word "lens".

We'll spend most of the remainder of this post discussing how to compute
`p(topic|word)`. Along the way, we'll come up with a clever representation of
our words, make some simplifying assumptions, and the come up with a fast and
scalable algorithm to recover these probabilities. However, once we have this
algorithm, it is a simple matter to multiply `p(word) * p(topic|word)` to
recover the topic-word distributions `p(word|topic)` that we were after in the
first.

## Cooccurrence Space

Q construction and meaning

## Anchor word selection

Choose crazy stuff with span max distances

## Anchor word reconstruction

C_j = argmin_C KL(\prod_i C_i a_i | Q_j)

## Topic-word distributions

Finally we use Bayes' Rule to get topics
