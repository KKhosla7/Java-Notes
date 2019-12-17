# Java 8 Notes

## Lambdas

* One can put modifiers on the parameters of a Lambda Expression
	* The Final Keyword
	* Annotations

* It is not possible to specify the returned type of a lambda expression.

## Method References

An alternative syntax fot Lambda Expressions.

## Lambda Expression vs Method Reference Examples

### Example#1:

#### Lambda Expression

```
Function<Person, Integer> f = person -> person.getAge();
```

#### Method Reference

```
Function<Person, Integer> f = Person::getAge;
```

### Example#2:

#### Lambda Expression

```
BinaryOperator<Integer> sum = (i1, i2) -> i1 + i2;
							= (i1, i2) -> Integer.sum(i1, i2);
```

#### Method Reference

```
BinaryOperator<Integer> sum = Integer::sum;
```

## Lambda Expressions

 1. So far a **lambda expression** is a new syntax.
 2. And a new way of writing instances of **anonymous classes**.
 3. **An alternative syntax**: Method Reference

> **How to create New API** - Lambdas + default methods + static methods

## Functional Interfaces

> A **lambda expression** is an instance of a functional interface.

## Java 8 - Under the Hood

The Java 8 Compiler is smart!

 - The interface is functional, so there is only one method to implement
 - The type of the variable gives the type of the **lambda expression**
 - The parameters and return types must be compatible
 - The same for the exception, if any

> A lambda expression is still an implementation of an interface.

```
Predicate<String> p = new Predicate<String>() {
	public boolean test(String s) {
		return s.length() < 20;
	}
}
```

```
Predicate<String> predicate = s -> s.length() < 20;
```

```
System.out.println(predicate.test("Hello World"));
```
	
## Defination:FunctionalInterface

* A Functional Interface is an interface:
	* With only one **abstract** method
	* default methods do not count
	* static methods do not count
	* Methods from the **Object** class do not count
	* Methods declared in **Object** class cannot be declared with default and static keywords. It will generate compile time error.
* A functional interface may be annotated with `@FunctionInterface`
	* It is not mandatory, for legacy reasons
	* The compiler will tell us if an annoted interface is functional or not.

## The java.util.function

* A new package from Java 8, with the most useful functional interfaces.
* There are 43 functional interfaces defined in the package.
* Four Categories -
	* The Consumers
	* The Suppliers
	* The Functions
	* The Predicates

## The Consumers

A Consumer consumes an object, and does not return anything

```
public interface Consumer<T> {
	public void accept(T t);
}
```

#### Example:

```
Consumer<String> printer = s -> System.out.println(s);
						 = System.out::println
```

## The Suppliers

A Supplier provides an object, takes no parameter.

> The **Supplier** is just an opposite of a **Consumer**

```
public interface Supplier<T> {
	public T get();
}
```

#### Example:

```
Supplier<Person> personSupplier = () -> new Person();
								= Person::new;
```

## The Functions

A Function takes an object and returns another object.

```
public interface Function<T, R> {
	public R apply(T t);
}
```

```
Function<Person, Integer> ageMapper = person -> person.getAge();
									= Person::getAge;
```
### UnaryOperator

```
public interface UnaryOperator<T> extends Function<T, T> {
}
```

A Unary Operator does not define any method, it is an interface that just extends function<T, T>, so a UnaryOperator takes an object of a given type and returns an object of the same type that might be the same object or a different object.

### BinaryOperator

```
public interface BinaryOperator<T> extends BiFunction<T, T, T> {
}
```

BinaryOperator that extends BiFucntion<T, T, T> takes two objects of the same type and returns one object of the same type as the parameters.

## The Predicates

A predicate takes an object and returns a boolean.

```
public interface Predicate<T> {
	public boolean test(T t);
}
```

```
Predicate<Person> ageGT20 = person -> person.getAge() > 20;
```

## Function Interfaces for Primitive Types

* Other functional interfaces have been defined, for instance:
	* IntPredicate
	* IntFunction
	* IntToDoubleFunction

## Iterable & Collection Interfaces

* On Iterable

```
// On Iterable
boolean forEach(Consumer<? super E> consumer;
```

* On Collection

```
// On Collection
boolean removeIf(Predicate<? super E> filter);
```

* Example:

```
List<Person> people = ... ;
people.forEach(System.out::println);

people.removeIf(person -> person.getAge() < 20);
```

