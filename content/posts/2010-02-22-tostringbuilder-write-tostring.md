---
title: 'Using ToStringBuilder to easily write toString() method'
date: 2010-02-22T00:00:00-07:00
tags: ["java"]
summary: "Apache's Commons Lang's ToStringBuilder facilitates easy toString() method creation."
---

Another useful class in Apache’s [Commons Lang](http://commons.apache.org/lang/) library is the `ToStringBuilder` class. I’ve already covered the `HashCodeBuilder` and the `EqualsBuilder` classes in previous posts. The `ToStringBuilder` class provides methods to easily write the `toString()` method in your class. The program below explains how to use it:

```java
public class Account implements Comparable {
  private long id;
  private String firstName;
  private String lastName;
  private String emailAddress;
  private Date creationDate;

  public String toString() {
    return new ToStringBuilder(this)
        .append("id", this.id)
        .append("firstName", this.firstName)
        .append("lastName", this.lastName)
        .append("emailAddress", this.emailAddress)
        .append("creationDate", this.creationDate)
        .toString();
  }
}
```

The output is of the form `Account@43d1[id=1, firstName=Parambir, lastName=Singh, emailAddress=test@parambir.com, creationDate=Mon Feb 22 10:54:29 IST 2010]`.

First we create an object of the `ToStringBuilder` class (and pass the `this` reference to the constructor). Then we use the `append` method to add each field that we want to print in the `toString()` method. Append method takes two arguments: `fieldName` and `fieldValue`. e.g. `builder.append("name", this.name)`. If the value of the field `name` was "Param", it will be printed as `name=Param`. The `append` method returns an instance of the `ToStringBuilder`, so the calls to multiple append methods can be chained (as in the example above).

Some other useful methods of the class are:

- `append(value)` – No field name is printed in this case, only the value is printed.
- `append(name, value)` – The field will be printed in the form `name=value`.
- `appendSuper(String s)` – Appends the `toString` from the super class.

The class also takes care of printing arrays properly!
