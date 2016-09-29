---
layout: post
title:  "{name}, Data Deduplication"
date:   2016-09-06 12:30:00
categories: python go data deduplication
---

Data deduplication

This is a layer that sits between the application and the storage media.

Basically it accepts a path as an input and return an array of chunks that needs
to be stored and metadata about the operation.


### Speed

{name} won't read unmodified files and we it does it will only read the bits that
from the previous run to the current one.


### Algorithm

https://en.wikipedia.org/wiki/Rolling_hash#Cyclic_polynomial


### Metadata


### Hashing


### Indexes


### Caches


### Language

Go is very well suited for this kind of operation due to the async I/O support.
