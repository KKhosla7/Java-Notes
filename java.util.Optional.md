# java.util.Optional\<T\>
	is designed to communicate to the user when a returned value may legitimately be null.

While Many developers assume that the goal of Optional is to remove `java.lang.NullPointerException` from your code, that’s not its real purpose.

## Two States of an Optional Object:
An instance of an Optional can be in one of two states:

1. a reference to an instance of type T, or
2.  empty

The former case is called _present_, and the latter is known as _empty_ (as opposed to _null_).

## Creating an Optional

The static factory methods to create an Optional are _empty_, _of_, and _ofNullable_, whose signatures are:

```
static <T> Optional<T> empty()
```

```
static <T> Optional<T> of(T value)
```

The wrapped value inside an Optional cannot be _null_ (in case of _primitives_) than its better to call `Optional.of` method when creating an Optional instance.

```
static <T> Optional<T> ofNullable(T value)
```

check if the contained value is _null_, and if it is return an _empty_ Optional, otherwise use `Optional.of` to wrap it.

## Two ways to modify the state of wrapped _mutable_ object inside an Optional

If the contained value inside an Optional is _mutable_ you can modify it with either the original reference, or one retrieved by calling get on the Optional.

## Extract a contained value from an Optional
* Optional.get

Never call get on an Optional If you’re not 100% sure the value is always present else `java.util.NoSuchElementException` is thrown If the Optional is _empty_.

* Optional.isPresent

Safely retrieves value from an Optional. Call isPresent to verify the value is _present_ or _absent_ before calling get. While this works, we instead of checking for `object == null` we are using `Optional.isPresent`, which doesn’t feel like much improvement.

* One of the variation of Optional.orElse

Fortunately, there’s a good alternative, which is to use the very convenient orElse method. orElse method returns the contained value if one is present, or a supplied default otherwise. It’s therefore a convenient method to use if you have a fallback value in mind.

### Few variations of orElse methods:

		* orElse(T other) returns the value if present, otherwise it returns the default value, other
		* orElseGet(Supplier<? extends T> other) returns the value if present, otherwise it invokes the Supplier and returns the result
		* orElseThrow(Supplier<? extends X> exceptionSupplier) returns the value if present, otherwise throws the exception created by the supplier

The _difference_ between _orElse_ and _orElseGet_ is that in case of _orElse_ the object passed as parameter is always created, whether the value exists in the Optional or not, while the latter uses a **Supplier**, which is only executed if the Optional is empty. **Hence prefer orElseGet is case complex object needs to be created**.

### Example:

```
List<ComplexObject> values = new ArrayList<>();
Optional<ComplexObject> value = values.stream().findFirst();

value.orElse(new ComplexObject());			// Always creates the new object
value.orElseGet(() -> new ComplexObject());		// Only creates object fi necessary
```


The _orElseThrow_ method also takes a _Supplier_, the _Supplier_ argument isn’t executed when the Optional contains a value.

### Example:

```
Optional<Integer> firstEvenNumber = Stream.of(1, 3, 5, 7, 9).filter(x -> x % 2 == 0).findFirst();
System.out.println(firstEvenNumber.orElseThrow(NoSuchElementException::new));
```

* Optional.ifPresent

Finally, the ifPresent method allows you to provide a Consumer that is only executed when the Optional contains a value.

### Example:

```
Optional<Integer> firstEvenNumber = Stream.of(1, 3, 5, 7, 9).filter(x -> x % 2 == 0).findFirst();
firstEvenNumber.ifPresent(val -> System.out.println(“Found first even number”));
```

## Optional in Getters and Setters

Wrap the result of _getter_ methods in Optionals, but do not do the same for setters, and especially not for attributes. **The Optional class, however, was deliberately designed not to be _serializable_,  so you don’t want to use it to wrap fields in a class.**

Consequently, the preferred mechanism for adding Optionals in getters and setters is to wrap nullable attributes in them when returned from getter methods, but not to do the same in setters.

### Example:

```
public class Department {
	private Manager boss;

	public Optional<Manager> getBoss() {
		return Optional.ofNullable(boss);
	}

	public void setBoss(Manager boss) {
		this.boss = boss;
	}
}
```

In Department, the Manager attribute boss is considered nullable. You might be tempted to make the attribute of type `Optional<Manager>`, but because _Optional is not serializable_, neither would be Department.

This approach used here is popular among open source developers who use ORM tools like Hibernate. It communicates the client that you’ve got a nullable database column backing this particular field, without forcing the client to wrap a reference in the setter as well.

This violates the “ **_JavaBeans_** ” pattern, the getter and setter are no longer symmetrical.

## Optional flatMap Versus map

_Avoid wrapping an Optional inside another Optional instead use the flatMap method in Optional._

The signature of the flatMap method in Optional is:

`<U> Optional<U> flatMap(Function<? super T, Optional<U>> mapper)`

## The API refers to Optional as a value-based class, meaning instances:

* Are final and immutable (though they may contain references to mutable objects)
* Have no public constructors, and thus must be instantiated by factory methods
* Have implementations of equals, hashCode, and toString that are based only on their state

## Where it makes sense to use an Optional:

* The Stream API, following methods return an Optional if no elements remain in the stream (values is filtered by some condition that happens to leave no elements remaining): reduce, min, max, findFirst, findAny.

## Things to remember:

* While Optional is a reference type, it should never be assigned a value of null. Doing so is a serious error
* Instances of Optional are immutable, but the objects they wrap may not be. The API refers to Optional as a value-based class
* Optional reference can be reassigned which basically says that immutable is not the same thing as final. What you can’t do is modify the Optional instance itself, because there are no methods available to do so
* The Optional class, however, was deliberately designed not to be serializable,  so you don’t want to use it to wrap fields in a class

> Using a Supplier as a method argument is an example of deferred or lazy execution. It allows you to avoid invoking the get method on the Supplier until necessary.  
