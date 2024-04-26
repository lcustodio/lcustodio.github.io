---
layout: post
title: "Kotlin Reified function - Generics"
excerpt: "How reified function helps on code readability"
tags: [syntax-sugar, reified, generics, kotlin]
comments: true
---

This post is based on a point Duncan McGregor made on this brilliant [video](https://www.youtube.com/watch?v=YKk2BRSXi6Y&list=PL1ssMPpyqocg6sa1ER-lrnQMBO9YiZfoY&index=1) about Data Oriented Programming (with gradual typing in Kotlin).

Java doesn't handle generics well - it's dense, it's verbose. It's java.
I was quick to assume that typescript would suffer from a similar pattern, but then... well no. It handles it fairly well:

## Kotlin code

The ugly:

```kotlin
val places = data["places"] as List<Map<String, Any?>>
```

The solution

```kotlin
inline fun <reified T> JsonObject.required(key: String) = get(key) as T

val places = data.required("places")
```

## Reified

Kotlin allows bytecode compilation with the same level of readability
Key safety is the type casting validation on runtime.
