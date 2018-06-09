---
layout:     post
title:      "Better Default Objects."
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

This eliminates much of the boilerplate methods that are common to many classes.

# Related

* [home]({{ site.baseurl }}/)
