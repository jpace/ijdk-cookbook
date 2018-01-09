---
layout:     post
title:      "Subclassing Array"
date:       2018-01-05 09:54:44 -0500
categories: array
---
The `Array` class can be subclassed, using some lesser-known Java generics behavior.

First, a bit of clarification: if you are subclassing, you most likely want to refer to `BaseArray`,
which is the actual class in which the `Array` behavior is defined.

The common `Array` class subclasses itself from `BaseArray` using a self-referential generic (this
does get a bit complicated):

```java
public class Array<T extends Object> extends BaseArray<T, Array<T>>
```

And `BaseArray`:

```java
public abstract class BaseArray<T extends Object, C extends BaseArray<T, C>>
    extends ArrayList<T>
    implements Sequence<T>
```

What all of that means is that `BaseArray` accepts a collection (of type `C`) that contains objects
of type `T`. And `BaseArray` extends `ArrayList`, with its elements being of type `T`.

Where `C` is relevant is that `BaseArray` has methods that return the same type of object as
`BaseArray`, even if subclassed.

For example, the `unique` (as well as others, including `append`, `elements`, `sorted`, `compact`,
`get(from, to)`) method returns an instance of type `C`:

```java
    public C unique() {
        C uniqueList = newInstance();
        for (T obj : this) {
            if (!uniqueList.contains(obj)) {
                uniqueList.append(obj);
            }
        }
        return uniqueList;
    }
```

The subclass of `BaseArray`, such as `Array`, is expected to define a method `newInstance` that
returns an object of that type, which in `Array` is:

```java
    public Array<T> newInstance() {
        return new Array<T>();
    }
```

Similarly, `IntegerList` has:

```java
    public IntegerList newInstance() {
        return new IntegerList();
    }
```

And is subclassed from `BaseArray` as:

```java
public class IntegerList extends BaseArray<Integer, IntegerList>
```

Again, that's telling `BaseArray` that the elements (`T`) are of type `Integer`, and the collection
itself is an `IntegerList`. That makes the following possible:

```java
    IntegerList ilist = IntegerList.of(2, 3, 2, 3, 3, 5, 5, 3)
    IntegerList result = ilist.unique();
    assertEqual(IntegerList.of(2, 3, 5), result, message("ilist", ilist));
```

That is one of the more powerful parts of IJDK, that subclasses retain their own references. In
comparison, the JDK `java.util.ArrayList#subList` method returns a `java.util.List`, making it less
valuable for anyone subclassing `ArrayList` and wanting `subList` to return a list of the same type
as the subclass.

Thus, one might want to use the `get(from, to)` method instead of `subList`:

```java
    IntegerList ilist = IntegerList.of(2, 3, 1, 7, 8));
    IntegerList result = ilist.get(1, -2);
    assertEqual(IntegerList.of(3, 1, 7), result, message("ilist", ilist));
```

This is complicated, but gives `BaseArray` subclasses flexibility that does not exist in the
equivalent JDK classes. The IJDK `Str` class is undergoing similar revisions, returning references
to `Str` instead of `java.lang.String`, which it wraps.

# Related

* [home]({{ site.baseurl }}/)
