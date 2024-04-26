---
layout: post
title: "Smooth loading of images"
excerpt: "Loading image ideas"
tags: [image-loading, batch]
comments: true
---

The idea is to discuss how to load a large amount of images in parallel focusing in smooth rendering

## Setup

By loading the images not in sequence, but using a algorithm similar to quick search.

So instead of loading like this

```
X X X - - - - - - - - -
```

It's possible to have a much responsive impact by loading like this:

```
X - - - - - X - - - - X
```
