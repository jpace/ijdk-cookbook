---
layout:     post
title:      "Using Range"
date:       2018-06-15 10:04:56 -0400
categories: life
---
`Range` is another class inspired by the class of the same name from Ruby. It is a pair of Integers,
a beginning and ending of a range.

At its most basic, a Range instance just refers to the first and last values:

```java
    // firstLast
    rg.toString();            // [3 .. 7]
    rg.first();               // 3
    rg.last();                // 7
    
    // iteration (inclusive)
    for (Integer num : rg) {
        // num                 : 3
        // num                 : 4
        // num                 : 5
        // num                 : 6
        // num                 : 7
    }

    // iteration (exclusive)
    for (Integer num : rg.upTo()) {
        // num                 : 3
        // num                 : 4
        // num                 : 5
        // num                 : 6
    }

    // includes
    rg.includes(4);           // true
    rg.includes(9);           // false
    
    // to array
    Array<Integer> ary = rg.toArray();
    ary;                      // [3, 4, 5, 6, 7]
    
    // comparison
    Range other = new Range(5, 10);
    // other               : [5 .. 10]
    rg.equals(other);         // false
    rg.compareTo(other);      // -1
```

# Related

* [home]({{ site.baseurl }}/)
