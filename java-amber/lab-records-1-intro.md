# Records, First Steps

## Introduction

This lab introduces you to the concept of records

Estimated Time: ~15 minutes

### **Objectives**

In this lab, you will:

* learn what records represent
* create a simple record
* observe the features it comes with

## Task 1: Immutable Data Classes

Classes are usually created to define behavior (like services) or mutable data structures (like lists), but often they just encode data (like a point with two coordinates) and frequently immutable data at that.
The Java language gives you several ways to create an immutable class.
Probably the most straightforward way is to create a final class with final fields and a constructor to initialize these fields.
Here is an example of such a class:

```java
<copy>
public class Point {
	private final int x;
	private final int y;

	public Point(int x, int y) {
		this.x = x;
		this.y = y;
	}
}
</copy>
```

But that's not all.
You probably need accessors for your fields, a `toString()` method and probably an `equals()` along with a `hashCode()` method.
Writing all this by hand is quite tedious and error-prone - IDEs help, of course, but adding or removing fields is still cumbersome.

More importantly, though, this fails to communicate what such class are about.
You wanted an immutable class that carries some data but nowhere is that idea easily recognizable.
Neither the compiler, nor the runtime, nor your colleagues can see that intention or benefit from it.

## Task 2: Records As Transparent Carriers Of Immutable Data

Records are here to help you make the previous code much simpler.
Starting with Java SE 14, you can write the following:

```java
<copy>
public record Point(int x, int y) {}
</copy>
```

What comes in parenthesis after the type name, here `int x` and `int y`, are called the record's _components_.
This single line of code creates the following elements for you:

1. A final class with two final fields: `x` and `y`, of type `int`.
2. A canonical constructor to initialize these two fields.
3. Two accessor methods `int x()` and `int y()` that return the corresponding fields' values.
4. Implementations of `toString()`, `equals()`, and `hashCode()` that use all fields.

As we will see later, you can modify much of this by adding your own implementations.

Records are making the creation of immutable aggregates of data much simpler, without the help of any IDE.
They reduce the risk of bugs because every time you modify the components of a record, the compiler automatically updates everything mentioned above, like adding it to `equals` and `hashCode` or removing an accessor.

Most importantly, a record clearly communicates to compiler, runtime, and coworkers that this type does not need encapsulation and acts as a transparent carrier for immutable data.

## Task 3: Your First Record

💪 To create your first record, create a file `Point.java` and add the following line:

```java
<copy>
public record Point(int x, int y) { }
</copy>
```

To experiment with it, create a second file `Main.java`, with a `main` method and construct some `Point` instances.
For example, you could print an instance to see the default `toString()` implementation in action or create two points with the same coordinates and observe that they are `equal`:

```java
<copy>
public class Main {

	public static void main(String[] args) {
		var pointA = new Point(0, 0);
		var pointB = new Point(0, 0);
		System.out.println(pointA + " equal " + pointB + "? ~> " + pointA.equals(pointB));
	}

}
</copy>
```

Congratulations on creating your first record!
Now let's see how to customize it.


## Learn More

* [Record documentation on dev.java](https://dev.java/learn/using-record-to-model-immutable-data/)


## Acknowledgements

* **Author** - [Nicolai Parlog, DevRel, Java Platform Group - Oracle](https://nipafx.dev/)
* **Contributor** - [José Paumard, DevRel, Java Platform Group - Oracle](https://twitter.com/JosePaumard)
* **Last Updated By/Date** - Nicolai Parlog, Sep. 19th 2022
