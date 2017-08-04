+++
title = "Anchor Words Algorithm Overview"
description = """Anchor words is a fast and scalable topic modeling \
algorithm, but the math is a bit dense. Here is a correct but simplified view \
of the algorithm."""
date = "2017-08-03"
draft = true
+++

## Preliminaries

It really isn't that bad. We make a big assumption but it works.
Don't really need to think about NMF

## Cooccurrence Space

Q construction and meaning

## Anchor word selection

Choose crazy stuff with span max distances

## Anchor word reconstruction

C_j = argmin_C KL(\prod_i C_i a_i | Q_j)

## Topic-word distributions

Finally we use Bayes' Rule to get topics
