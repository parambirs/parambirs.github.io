---
title: 'Using CompareToBuilder to implement Comparable interface'
date: 2010-02-27T00:00:00-07:00
tags: ["java"]
summary: "Apache's Commons Lang offers CompareToBuilder for streamlined Comparable implementation."
---

Many a times, we need to implement the [java.lang.Comparable](http://java.sun.com/j2se/1.4.2/docs/api/java/lang/Comparable.html) interface. This interface has a single method `int compareTo(Object o)`. Apache’s [Commons Lang library](http://commons.apache.org/lang/) provides a utility class – [CompareToBuilder](http://commons.apache.org/lang/api-2.4/org/apache/commons/lang/builder/CompareToBuilder.html) which makes it easy to write the `compareTo()` method. The method is consistent with the `equals()` and `hashCode()` methods built using `EqualsBuilder` and `HashCodeBuilder` (provided the same fields are used in all of them). A typical use of the class is as follows:

```java
public class Account implements Comparable<Account> {
  private long id;
  private String firstName;
  private String lastName;
  private String emailAddress;
  private Date creationDate;

  public int compareTo(Account account) {
    return new CompareToBuilder()
        .append(this.id, account.id)
        .append(this.firstName, account.firstName)
        .append(this.lastName, account.lastName)
        .append(this.emailAddress, account.emailAddress)
        .append(this.creationDate, account.creationDate)
        .toComparison();
  }
}
```

We first create an instance of the `CompareToBuilder`. The `append` method takes two arguments – the two fields we want to compare. It returns an instance of `CompareToBuilder` so multiple calls to the append method can be chained (as in the above example). Finally we call the `toComparison()` method which returns the appropriate value to the caller.