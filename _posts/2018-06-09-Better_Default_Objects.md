---
layout:     post
title:      "Better Default Objects"
date:       2018-06-09 11:11:53 -0400
categories: class
---

Over the past couple of weeks, IJDK has been expanded with some very cool behavior. A new interface,
HasInstanceValues, can be implemented by subclases, and static methods in the class Objects can
provide the implementation of `equals`, `hashCode`, and `toString`. Therefore, HasInstanceValues
subclasses only need to delegate to the same methods in Objects.

For example, in the `KeyValue` class (edited for brevity), 

```java
    public boolean equals(Object obj) {
        if (obj instanceof KeyValue) {
            KeyValue<?, ?> other = (KeyValue)obj;
            return Objects.equals(this, other);
        }
        else {
            return false;
        }
    }

    public int hashCode() {
        return Objects.hashCode(this);
    }

    public String toString(String sep) {
        return Objects.toString(this);
    }

    public Array<Object> getInstanceValues() {
        return Array.of(key, value);
    }
```

Objects#equals simply compares each element returned by `getInstanceValues`:

```java
    public static boolean equals(HasInstanceValues x, HasInstanceValues y) {
        if (x == null) {
            return y == null;
        }
        else if (y == null) {
            return false;
        }
        else {
            return Arrays.equals(getInstanceObjects(x), getInstanceObjects(y));
        }
    }
```

Similarly, `hashCode` and `toString` use the elements returned by `getInstanceObjects`:

```java
    public static int hashCode(HasInstanceValues x) {
        return x == null ? 0 : Arrays.hashCode(getInstanceObjects(x));
    }

    public static String toString(HasInstanceValues x) {
        return toString(x, ", ");
    }

    public static String toString(HasInstanceValues x, String delim) {
        return x == null ? null : x.getInstanceValues().join(delim);
    }
```

This eliminates much of the boilerplate methods that are common to many classes.

# Related

* [home]({{ site.baseurl }}/)
