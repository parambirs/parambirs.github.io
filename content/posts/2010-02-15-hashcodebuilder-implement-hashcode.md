---
title: 'Using HashCodeBuilder to implement hashCode()'
date: 2010-02-15T00:00:00-07:00
tags: ["java"]
summary: "Apache's Commons Lang offers HashCodeBuilder for streamlined and efficient hashCode() method implementation."
---

Similar to overriding `equals()`, you may also need to override the `hashCode()` method in your custom class. The `equals()` contract states that two objects which compare equal _must_ have the same hashCode. Moreover, for efficient storage in `HashMap`, non-equal objects _should_ return different hashcodes.

Since equal objects should have equal hashcodes, you must compare the same fields as in the `equals()` method.

With all these concerns in mind, writing an effective `hashCode()` method might be difficult. Fortunately, the Apache [Commons Lang](http://commons.apache.org/lang/) library provides a utility class called `HashCodeBuilder` which makes it fairly easy to implement an efficient `hashCode` method.

Here’s how you use this class:

```java
public class Account implements Comparable {
  private long id;
  private String firstName;
  private String lastName;
  private String emailAddress;
  private Date creationDate;

  public int hashCode() {
    return new HashCodeBuilder(11, 21)
        .append(this.id)
        .append(this.firstName)
        .append(this.lastName)
        .append(this.emailAddress)
        .append(this.creationDate)
        .toHashCode();
  }
}
```

First you need to initialize an object of the class `HashCodeBuilder`. The constructor takes two integers as arguments. You need to supply two random odd integers (preferable prime) as the arguments. You pass a relevant field of the class (that you want to be included while calculating the `hashCode`) to the `append` method. The method returns a `HashCodeBuilder` instance. So, you can chain the calls to the append methods together (as in the example above). Finally, you call the `toHashCode()` method which returns the computed hash code to the caller.

Some other useful constructors/methods in the class are:

- `appendSuper(int)` – use this method if you want to append the `hashCode` of the super class object. E.g. `o.appendSuper(super.hashCode());`
- `HashCodeBuilder(int initialNonZeroOddNumber, int multiplierNonZeroOddNumber)`: These two arguments are used for calculating the `hashCode` for the object. Try to use different arguments for different classes.
- `HashCodeBuilder()`: This uses two hard coded choices for the constants for calculating the `hashCode`.
- `hashCode()`: returns the computed hash code (same as `toHashCode()`). Note: It doesn’t return the `hashCode` of the `HashCodeBuilder` object.

