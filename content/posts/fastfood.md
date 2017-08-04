+++
date = "2014-12-02"
title = "Why Fast Food Succeeds"
description = """After running some supervised topic modeling experiments on \
Yelp data, here is what I've discovered about why people hate certain \
restaurants."""
draft = true
+++

## Is Your Anchor Going Up or Down?

I'm an author on a rather interesting paper:
[Is Your Anchor Going Up or Down? Fast and Accurate Supervised Topic Modeling]({{< relref "publications/supank_naacl15.md" >}}).
Topic modeling is a class of algorithms which automatically extract general
themes or topics from text. For example, in a collection of book reviews, you
might find a topic about "vampire romance." However, sometimes you also want to
include some notion of supervision in the model. For example, if you included
sentiment ratings into your topic model, you might discover a vampire romance
topic with terms like "Buffy" and "Spike" for positive sentiment and a topic
about vampire romance with terms like "Bella" and "Edward" having negative
sentiment (I haven't read them, so I can only assume this is what would
happen).

Our paper basically gives a way to do this sort of topic modeling
using an extension to the
[Anchor Algorithm](https://arxiv.org/pdf/1212.4777.pdf).
I won't bore you with the details, but basically our algorithm finds "anchor
words" to anchor each topic. For example the word "twilight" might anchor our
vampire topic associated negative sentiment.

## Supervised Anchors on Yelp

Okay so we can do supervised topic modeling using some nifty anchor math. What
amusing observations can we now make? One interesting experiment was running
this algorithm on a bunch of Yelp restaurant reviews. Unsurprisingly, most of
the topics were related to various types of food such as pizza, burgers or
steak. Other topics were dedicated to specific cuisines such as French cuisine
or Thai cuisine. There were even topics discussing drinks such as beer and
wine.

Things got a little bit more interesting once we added in sentiment in the form
of the star ratings from the Yelp reviews. With sentiment added, we can start
to see topics which carry some notion of sentiment. We can then look at those
particular topics and see what makes or breaks a restaurant on Yelp.

Topics with positive sentiment tended to talk about the food. Words like
"love", "favorite", "amazing" and "delicious" appear in these positive topics.
This isn't too terribly surprising. What was surprising was the contents of the
most negative topics. I would have guessed that negative reviews would talk
about the quality of the food using words like "gross" or "disgusting".
Instead, the words associated with the most negative topics were things like
"line", "wait", "long", "waiting" or "minutes."

So what does this tell us? According to this analysis, the worst thing you can
do for your ratings on Yelp is to have slow service. I suppose this actually
matches the last review I personally left which read something like "While the
food was good, the service was terrible and we waited over 40 minutes for our
food to arrive." I guess I now understand McDonald's success - cheap crappy
food but at least it is fast.
