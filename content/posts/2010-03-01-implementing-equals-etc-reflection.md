---
title: 'Implementing equals(), hashCode(), toString() & compareTo() using reflection'
date: 2010-03-01T00:00:00-07:00
tags: ["java"]
summary: "Apache Commons Lang simplifies method implementation (equals(), hashCode(), etc.) with reflection, albeit with slower performance compared to explicit field usage."
---

In my previous posts, I wrote about the various utility classes provided by Apache’s commons lang library for implementing commonly used methods (`equals()`, `hashCode()` etc.). All of these classes also provide the option of implementing these methods using reflection. That reduces your code for implementing any of these methods to just writing one line! For example, to implement `toString()` method using reflection, use the `reflectionToString()` static method of the `ToStringBuilder` class. Here’s the implementation of all the above methods using reflection in a sample class:

```java
public class Account implements Comparable {
  private long id;
  private String firstName;
  private String lastName;
  private String emailAddress;
  private Date creationDate;

  public boolean equals(Object obj) {
    return EqualsBuilder.reflectionEquals(this, obj);
  }

  public int hashCode() {
    return HashCodeBuilder.reflectionHashCode(this);
  }

  public int compareTo(Account account) {
    return CompareToBuilder.reflectionCompare(this, account);
  }

  public String toString() {
    return ToStringBuilder.reflectionToString(this);
  }
}
```

However, these methods use `AccessibleObject.setAccessible()` method to change the visibility of class fields (since generally these fields are private). This will not work under a security manager, unless appropriate permissions are set up correctly. Also, these methods are slower than using the fields explicitly.