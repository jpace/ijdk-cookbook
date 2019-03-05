---
layout:     post
title:      "Using Iterate"
date:       2019-03-05 15:49:13 -0500
categories: collections
---
The Iterate class handles many common cases of iterating over a collection, such as gracefully
avoiding dereferencing null objects, executing a block a number of times, and combining an index and
object as the element being iterated over.

# Count

A basic and common case is `count`, which simply executes a number of times:

```java
    // count - old
    for (int i = 0; i < 3; ++i) {
        println("i", i);
    }
    // i           : 0
    // i           : 1
    // i           : 2
        
    // count - new
    for (Integer i : Iterate.count(3)) {
        println("i", i);
    }
    // i           : 0
    // i           : 1
    // i           : 2
```

# Handling Null References

`Iterate` methods handle null collection and array instances as if they were empty, and does not
execute the associated block.

```java
    String[] ary = null;

    // over null array - old
    if (ary != null) {
        for (String s : ary) {
            println("s", s);
        }
    }
    
    List<String> list = null;

    // over null iterable - old
    if (list != null) {
        for (String s : list) {
            println("s", s);
        }
    }
        
    // over null array - new
    for (String s : Iterate.over(ary)) {
        println("s", s);
    }
    
    // over null iterable - new
    for (String s : Iterate.over(list)) {
        println("s", s);
    }
```

# Skipping Null Elements

Elements within an array or collection that are null can be skipped.

```java
    List<String> list = Arrays.asList("a", null, "c");

    // over non null - old
    for (String s : list) {
        if (s != null) {
            println("s", s);
        }
    }

    // over non null - new
    for (String s : Iterate.overNonNull(list)) {
        println("s", s);
    }
```

# Using Indices and Elements

A `KeyValue` instance can be used when using both the index (the key) and the element (the value)
when iterating. As with other `Iterate` methods, `eachWithIndex` safely handles null.

```java
    String[] ary = new String[] { "a", "b", "c" };

    // each with index, array - old
    int index = 0;
    for (String s : ary) {
        println("index", index);
        println("s", s);
        ++index;
    }
    // index       : 0
    // s           : a
    // index       : 1
    // s           : b
    // index       : 2
    // s           : c
    
    // each with index, array - new
    for (KeyValue<Integer, String> it : Iterate.eachWithIndex(ary)) {
        println("it.key()", it.key());
        println("it.value()", it.value());
    }
    // it.key()    : 0
    // it.value()  : a
    // it.key()    : 1
    // it.value()  : b
    // it.key()    : 2
    // it.value()  : c
```

# Preview

The next version of `Iterate` will include more filtering for object values.

# Related

* [home]({{ site.baseurl }}/)
