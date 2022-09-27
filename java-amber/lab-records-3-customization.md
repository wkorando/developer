# Records, More Customization

## Introduction

This lab explores more ways to customize records

Estimated Time: ~20 minutes

### **Objectives**

In this lab, you will learn how to:

* override the various methods that records provide by default
* add additional behavior to a record

## Task 1: Override Accessors

As mentioned before, a record generates the following methods:

* for each component, a method with the same name that returns the field's value (called an _accessor_; e.g. `start()` for `start`)
* `equals(Object)` that uses all fields
* `hashCode()` that uses all fields
* `toString()` that uses all fields

All of these methods can be reimplemented in code to change their behavior.
This is not typically advisable for accessors because records should be _transparent_ carriers of (immutable) data, which means the accessor should return the field's value as-is.
A possible exception is a record that has a mutable collection as field (which is in itself questionable because, ideally, a record is _immutable_) but wants to return an unmodifiable view on it.

As an exercise, take the following `Node` that can form a tree (if cycles are prevented):

```java
<copy>
public record Node(int id, List<Node> children) {

	public Node {
		children = new ArrayList<>(children);
	}

}
</copy>
```

💪 Note how its compact constructor ensures that `children` is a mutable list, probably to later offer explicit methods that update it.
If `Node` wants to make sure that no outside code can directly update the list, it needs to implement an accessor that returns an unmodifiable view or an immutable collection.
Give that a try.

The solution looks as follows:

```java
<copy>
@Override
public List<Node> children() {
	return Collections.unmodifiableList(children);
}
</copy>
```

## Task 2: Override `equals`, `hashCode`, or `toString`

For classes, the reason to implement `equals`, `hashCode`, or `toString` is to create domain-specific behavior, for example that instances of a `User` class can have meaningful equality for collection operations.
For records, this behavior is predefined by their implementations of these methods.
But the default implementation won't always be the best one and so it is easy to create custom behavior for these methods just by overriding them.

Take the `Node` record as an example.
Its implementations of `equals`, `hashCode`, and `toString` include the `children`, which means the same methods will be called on them and on their children and so forth.
Calling `toString()` on the tree's root will print the entire tree, which may be desirable, but often isn't.

💪 As an exercise, override these three methods with ones that only use the node's `id` field.

The solution should look something like this:

```java
<copy>
@Override
public boolean equals(Object o) {
	if (this == o)
		return true;
	if (!(o instanceof Node node))
		return false;
	return id == node.id;
}

@Override
public int hashCode() {
	return Objects.hash(id);
}

@Override
public String toString() {
	return "Node[id=" + id + ']';
}
</copy>
```

### Sidetrack

Look, this lab is about records, but there's one thing that you need to see!
You've probably heard about type patterns with `instanceof` - they make implementing `equals` so much easier!
Take a look at this:

```java
<copy>
@Override
public boolean equals(Object o) {
	return this == o
			|| o instanceof Node node
			&& id == node.id;
}
</copy>
```

If you're not sure why that works, learn up on type patterns.
But note that checking the type with `instanceof` is different from `getClass()` with regards to inheritance, so if `instanceof` is used, either `equals` or the class should be final.
This is fulfilled because `Node` is a record, which are always implicitly final.


## Task 3: Add More Methods

Aside from a few restrictions, like limited inheritance, a record works just like a class and so it is possible to add methods.
As a rule of thumb, it's usually a good idea to limit them to their own fields or those of other instances of the record, but that's "just" a design concern.

💪 As an exercise, create a method that clears the list of children.

Your solution might look as follows:

```java
<copy>
public void removeChildren() {
	children.clear();
}
</copy>
```


## Task 4: Implement Interfaces

Just like all enums implicitly inherit from `java.lang.Enum`, all records implicitly inherit from `java.lang.Record`
Records can hence not extend other classes but they can implement interfaces just like classes do.

💪 As an exercise, extend the `Node` record to implement `Comparable<Node>`, so that nodes are compared by their `id`.

Your solution should look something like this:

```java
<copy>
public record Node(int id, List<Node> children) implements Comparable<Node> {

	// [...]

	@Override
	public int compareTo(Node other) {
		return this.id - other.id;
	}

}
</copy>
```


## Learn More

* [Record documentation on dev.java](https://dev.java/learn/using-record-to-model-immutable-data/)
* [JEP 394: Pattern Matching for instanceof](https://openjdk.org/jeps/394)
* [How To Implement `equals` Correctly](https://nipafx.dev/implement-java-equals-correctly/)

## Acknowledgements

* **Author** - [Nicolai Parlog, DevRel, Java Platform Group - Oracle](https://nipafx.dev/)
* **Last Updated By/Date** - Nicolai Parlog, Sep. 21st 2022
