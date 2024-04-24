---
title: 'Using EqualsBuilder to create equals() method'
date: 2010-02-13T00:00:00-07:00
tags: ["java"]
summary: "Apache's Commons Lang's EqualsBuilder simplifies proper equals() method implementation."
---

While writing you own classes, you generally need to implement `equals()` and `hashCode()` methods. Also, you need to implement the methods such that objects which are equal must generate the same hashCode. The contract for `equals()` method (from the documentation for `Object` class) states that:

> The equals method implements an equivalence relation on non-null object references:
>
> - It is reflexive: for any non-null reference value `x`, `x.equals(x)` should return `true`.  
> - It is symmetric: for any non-null reference values `x` and `y`, `x.equals(y)` should return `true` if and only if `y.equals(x)` returns `true`.  
> - It is transitive: for any non-null reference values `x`, `y`, and `z`, if `x.equals(y)` returns `true` and `y.equals(z)` returns `true`, then `x.equals(z)` should return `true`.  
> - It is consistent: for any non-null reference values `x` and `y`, multiple invocations of `x.equals(y)` consistently return `true` or consistently return `false`, provided no information used in equals comparisons on the objects is modified.  
> - For any non-null reference value `x`, `x.equals(null)` should return `false`.  

So, in your implementation, for all the relevant fields you need to do the following:

1. Check that the field is not `null`
2. Compare the fields in the two objects properly:
     - If field is of primitive type, use `==`
     - If it is of some class type, use `equals()`
     - If it is of array type, use `Arrays.equal()`

Writing all this is not hard, but it is tedious. The `EqualsBuilder` class from the Apache’s [Commons Lang](http://commons.apache.org/lang/) project makes it quite easy to properly implement the `equals()` method in your class. Typical use is as follows:

```java
public boolean equals(Object obj) {
   if (obj == null) { return false; }
   if (obj == this) { return true; }
   if (obj.getClass() != getClass()) {
     return false;
   }
   MyClass rhs = (MyClass) obj;
   return new EqualsBuilder()
                 .appendSuper(super.equals(obj))
                 .append(field1, rhs.field1)
                 .append(field2, rhs.field2)
                 .append(field3, rhs.field3)
                 .isEquals();
}
```

We create an instance of the `EqualsBuilder` class and we supply it the pairs of fields that we want to compare (e.g. `field1` & `rhs.field1`). The `append()`/`appendSuper()` methods return an instance of `EqualsBuilder` itself, so calls to multiple append methods can be chained as shown above. Finally, we return the value from the `isEquals()` method of the last instance in the chain.

A complete example follows:

```java
public class Account {
  private long id;
  private String firstName;
  private String lastName;
  private String emailAddress;
  private Date creationDate;

  public boolean equals(Object obj) {
    if(this == obj) {
      return true;
    }
    if(obj == null || this.getClass() != obj.getClass()) {
      return false;
    }
  
    Account account = (Account) obj;
    return new EqualsBuilder().append(this.id, account.id)
      .append(this.firstName, account.firstName)
      .append(this.lastName, account.lastName)
      .append(this.emailAddress, account.emailAddress)
      .append(this.creationDate, account.creationDate)
      .isEquals();
  }
}
```
