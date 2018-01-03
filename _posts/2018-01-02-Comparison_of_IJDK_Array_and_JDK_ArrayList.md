---
layout:     post
title:      "Comparison of IJDK Array and JDK ArrayList"
date:       2018-01-02 09:18:21 -0500
categories: array
---
The most heavily used class from IJDK, possibly other than `Str`, is `Array`. This post compares the
implementations of common idioms, using the JDK `java.util.ArrayList` and
`org.incava.ijdk.collect.Array` classes.

# Creating an Empty Array

`Array` has short syntax for creating an empty object.

```java
    ArrayList<Integer> list = new ArrayList<>();

    // more explicit that the array is empty (but mutable);
    // no need to use <> or <Integer>:
    Array<Integer> ary = Array.empty();
```

# Creating an Array with Elements

`Arrays.asList(...)` returns an immutable `ArrayList`. The IJDK `Array` is always mutable, and uses
a short `of` method.

```java
    // Arrays.asList returns an immutable array, so ArrayList has to wrap it:
    ArrayList<Integer> list = new ArrayList<>(Arrays.asList(3, 17, 212));

    // how concise
    Array<Integer> ary = Array.of(3, 17, 212);
    
    // list        : [3, 17, 212]
    // ary         : [3, 17, 212]
```

# Get

As in other programming languages such as Perl and Ruby, a negative index with an array means to get
that element, starting at the end of the list. The `IndexOutOfBoundsException` is not thrown, but
`null` is returned instead.

```java
    ArrayList<Integer> list = new ArrayList<>(Arrays.asList(60, 16, 252, 9, 3, 17));
    Array<Integer> ary = Array.of(60, 16, 252, 9, 3, 17);
    // list/ary    : [60, 16, 252, 9, 3, 17]
    
    Integer n;

    n = list.size() > 0 ? list.get(list.size() - 1) : null;
    n = ary.get(-1);
    // n           : 17
    
    n = list.size() > 0 ? list.get(list.size() - 2) : null;
    n = ary.get(-2);
    // n           : 3
    
    n = list.size() >= 8 ? list.get(8) : null;
    n = ary.get(8);
    // n           : null
    
    n = list.size() > 8 ? list.get(list.size() - 8) : null;
    n = ary.get(-8);
    // n           : null    
```

# Append (Add)

This allows the chaining of methods, for more concise code.

```java
    ArrayList<Integer> list = new ArrayList<>(Arrays.asList(3, 7, 23, 50));
    Array<Integer> ary = Array.of(3, 7, 23, 50);
    // list/ary    : [3, 7, 23, 50]

    list.add(2);
    list.add(8);
    list.add(6);
    
    ary.append(2).append(8).append(6);
    // list/ary    : [3, 7, 23, 50, 2, 8, 6]
```

# First, Last

Among the most common methods on an array are getting the first and last element, and removing them.

```java
    ArrayList<Integer> list = new ArrayList<>(Arrays.asList(60, 16, 252, 9, 3, 17));
    Array<Integer> ary = Array.of(60, 16, 252, 9, 3, 17);
    // list/ary    : [60, 16, 252, 9, 3, 17]

    Integer n;
    
    n = list.size() > 0 ? list.get(0) : null;
    n = ary.first();
    // n           : 60

    n = list.size() > 0 ? list.get(list.size() - 1) : null;
    n = ary.last();
    // n           : 17

    n = list.size() > 0 ? list.remove(0) : null;
    n = ary.takeFirst();
    // n           : 60
    // list/ary    : [16, 252, 9, 3, 17]

    n = list.size() > 0 ? list.remove(list.size() - 1) : null;
    n = ary.takeLast();
    // n           : 17
    // list/ary    : [16, 252, 9, 3]
```

# Sorting

Unlike the JDK implementation, one can create a sorted list from another, without changing the
original list.

```java
    ArrayList<Integer> list = new ArrayList<>(Arrays.asList(60, 16, 252, 9, 3, 17));
    Array<Integer> ary = Array.of(60, 16, 252, 9, 3, 17);
    // list/ary    : [60, 16, 252, 9, 3, 17]

    ArrayList<Integer> slist = new ArrayList<>(list);
    Collections.sort(slist);

    Array<Integer> sary = ary.sorted();
    // slist/sary  : [3, 9, 16, 17, 60, 252]
```

# Unique Elements

Inspired by Ruby `Array#uniq`, it is trivial to extract an Array of unique elements.

```java
    List<Integer> list = new ArrayList<>(Arrays.asList(11, 13, 11, 2, 4, 8, 11, 8));
    Array<Integer> ary = Array.of(11, 13, 11, 2, 4, 8, 11, 8);
    // list/ary    : [11, 13, 11, 2, 4, 8, 11, 8]

    List<Integer> ulist = new ArrayList<>();
    for (Integer it : list) {
        if (!ulist.contains(it)) {
            ulist.add(it);
        }
    }
    Array<Integer> uary = ary.unique();
    // ulist/uary  : [11, 13, 2, 4, 8]
```

# Join

Joining elements into a string is easy.

```java
    List<Integer> list = new ArrayList<>(Arrays.asList(60, 16, 252, 9, 3, 17));
    Array<Integer> ary = Array.of(60, 16, 252, 9, 3, 17);        
    // list/ary    : [60, 16, 252, 9, 3, 17]

    StringBuilder sb = new StringBuilder();
    List<Integer> ulist = new ArrayList<>();
    boolean isFirst = true;
    for (Integer it : list) {
        if (isFirst) {
            isFirst = false;
        }
        else {
            sb.append(", and ");
        }
        sb.append(String.valueOf(it));
    }
    String slist = sb.toString();
    String sary = ary.join(", and ");
    // slist/sary  : 60, and 16, and 252, and 9, and 3, and 17
```

# Plus

Array makes it simple to create a new Array from an existing array and another collection.

```java
    List<Integer> list = new ArrayList<>(Arrays.asList(60, 16, 252, 9, 3, 17));
    Array<Integer> ary = Array.of(60, 16, 252, 9, 3, 17);        
    // list/ary    : [60, 16, 252, 9, 3, 17]

    List<Integer> toAdd = Arrays.asList(9, 6, 3, 1);

    ArrayList<Integer> plist = new ArrayList<>(list);
    plist.addAll(toAdd);

    Array<Integer> pary = ary.plus(toAdd);
    // plist/pary  : [60, 16, 252, 9, 3, 17, 9, 6, 3, 1]
```

# Minus

Creating an Array from an existing Array, removing those elements found in another collection.

```java
    List<Integer> list = new ArrayList<>(Arrays.asList(3, 1, 4, 8, 2, 9));
    Array<Integer> ary = Array.of(3, 1, 4, 8, 2, 9);
    // list/ary    : [3, 1, 4, 8, 2, 9]

    List<Integer> toRemove = Arrays.asList(2, 4, 6, 8);
    ArrayList<Integer> mlist = new ArrayList<>(list);
    mlist.removeAll(toRemove);

    Array<Integer> mary = ary.minus(toRemove);
    // mlist/mary  : [3, 1, 9]
```

# Intersection

A new array, containing the common elements of an array and a collection.

```java
    List<Integer> list = new ArrayList<>(Arrays.asList(3, 1, 4, 8, 2, 9));
    Array<Integer> ary = Array.of(3, 1, 4, 8, 2, 9);
    // list/ary    : [3, 1, 4, 8, 2, 9]
    
    List<Integer> other = Arrays.asList(2, 4, 6, 8);

    List<Integer> ilist = new ArrayList<>();
    for (Integer it : list) {
        if (other.contains(it)) {
            ilist.add(it);
        }
    }

    Array<Integer> iary = ary.intersection(other);
    // ilist/iary  : [4, 8, 2]
```

# Elements

A way to extract individual elements into a new `Array`.

```java
    List<Integer> list = new ArrayList<>(Arrays.asList(3, 1, 4, 8, 2, 9));
    Array<Integer> ary = Array.of(3, 1, 4, 8, 2, 9);
    // list/ary    : [3, 1, 4, 8, 2, 9]

    List<Integer> elist = new ArrayList<>();
    elist.add(list.size() > 3  ? list.get(3) : null);
    elist.add(list.size() > 0  ? list.get(0) : null);
    elist.add(list.size() > 2  ? list.get(list.size() - 2) : null);
    elist.add(list.size() > 3  ? list.get(3) : null);

    Array<Integer> eary = ary.elements(3, 0, -2, 3);
    // elist/eary  : [8, 3, 2, 8]
```

# Set

The `set` method dynamically resizes an array, in contrast to `java.util.ArrayList#set`, which
throws an `IndexOutOfBoundsException`.

```java
    List<Integer> list = new ArrayList<>(Arrays.asList(3, 1, 4, 8, 2, 9));
    Array<Integer> ary = Array.of(3, 1, 4, 8, 2, 9);
    // list/ary    : [3, 1, 4, 8, 2, 9]

    while (list.size() <= 8) {
        list.add(null);
    }
    list.set(8, 11);

    if (list.size() > 5) {
        list.set(list.size() - 5, 13);
    }
    else {
        throw new IllegalArgumentException("index: " + -5 + " is below the minimum " + -list.size());
    }

    ary.set(8, 11);
    ary.set(-5, 13);

    // list/ary    : [3, 1, 4, 8, 13, 9, null, null, 11]
```

# Get Range (Subset)

It's simple to get a subset, using the same negative indices as `get`.

```java
    List<Integer> list = new ArrayList<>(Arrays.asList(3, 1, 4, 8, 2, 9));
    Array<Integer> ary = Array.of(3, 1, 4, 8, 2, 9);
    // list/ary    : [3, 1, 4, 8, 2, 9]

    int from = 1;
    int to = list.size() - 2;
    List<Integer> rlist = new ArrayList<>();
    for (int idx = from; idx <= to; ++idx) {
        rlist.add(list.get(idx));
    }

    Array<Integer> rary = ary.get(1, -2);
    // rlist/rary  : [1, 4, 8, 2]
}
```

# Example

```java
    Array<String> simpsons = Array.of("homer", "marge", "lisa", "bart");
    // simpsons    : [homer, marge, lisa, bart]

    simpsons.append("grandpa").append("maggie");
    // simpsons    : [homer, marge, lisa, bart, grandpa, maggie]

    if (simpsons.containsAny("ned", "maude")) {
    }

    String homer = simpsons.get(0);
    // homer       : homer

    String maggie = simpsons.get(-1);
    // maggie      : maggie

    String bart = simpsons.get(-3);
    // bart        : bart

    String noone = simpsons.get(20);
    // noone       : null

    Array<String> adults = simpsons.elements(0, 1, -2);
    // adults      : [homer, marge, grandpa]

    Array<String> kids = simpsons.elements(2, 3, -1).plus(Array.of("nelson", "milhouse"));
    // kids        : [lisa, bart, maggie, nelson, milhouse]
    
    Array<String> sorted = simpsons.sorted();
    // sorted      : [bart, grandpa, homer, lisa, maggie, marge]

    simpsons.append("bart").append(null).append("homer").append("lisa").append(null);
    // simpsons    : [homer, marge, lisa, bart, grandpa, maggie, bart, null, homer, lisa, null]

    Array<String> unique = simpsons.unique();
    // unique      : [homer, marge, lisa, bart, grandpa, maggie, null]

    Array<String> nonNull = simpsons.compact();
    // nonNull     : [homer, marge, lisa, bart, grandpa, maggie, bart, homer, lisa]

    String joined = sorted.get(0, -2).join(", ") + " and " + sorted.get(-1);
    // joined      : bart, grandpa, homer, lisa, maggie and marge
```

> We think in generalities, but we live in detail. -- Alfred North Whitehead

# Related

* [home]({{ site.baseurl }}/)
