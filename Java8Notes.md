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

## List Interface

* On List

```
// On List
boolean replaceAll(UnaryOperator<? super E> operator);
```

```
// On List
boolean sort(Comparator<? super E> comparator);
```

* Example:

```
List<String> names = ... ;
names.replaceAll(name -> name.toUpperCase());
names.replaceAll(String::toUpperCase);
```

```
List<Person> people = ... ;
names.sort(
	Comparator.comparing(Person::getName)
			  .thenComparing(Person::getAge)
);
```

## Map Interface

```
// On Map
void forEach(BiConsumer<? super K, ? super V> consumer);
```
* Example:

```
Map<City, List<Person>> map = ... ;
map.forEach(
	(city, list) ->
	System.out.println(city + " : " + list.size() + " people")
);
```

* A newer version of the get() method

```
// On Map
V getOrDefault(Object key, V defaultValue);
```

* Allows one to check if a key is present in a map or not

```
Map<City, List<Person>> map = ... ;

System.out.println(map.getOrDefault(boston, emptyList()));
```

* New version of the put() method

```
// On Map
V putIfAbsent(K key, V value);
```

* Example:

```
Map<City, List<Person>> map = ... ;
map.putIfAbsent(boston, new ArrayList<>()); // returns the previous value
```

```
Map<City, List<Person>> map = ... ;
map.putIfAbsent(boston, new ArrayList<>());
map.get(boston).add(maria);
```
* New replace() method

```
// On Map
V replace(K key, V newValue);
boolean replace(K key, V existingValue, V newValue);
```

```
// On Map
void replaceAll(BiFunction<? super K, ? super V, ? extends V> function);
```

* New remove() method:

```
// On Map
void remove(Object key, Object value);
```

* A new family of methods:compute*()

```
// On Map
V compute(
	K key, BiFunction<? super K, ? super V, ? extends V> remapping);
```

* Computes a new value from:
	* the key passed as a parameter, that may not be in the map
	* the value that may be associated with that key, or null
	* the lambda that will compute the remapping

* The computeIfAbsent() version

```
// On Map
V computeIfAbsent(
	K key, Function<? super K, ? extends V> mapping);
```

* Computes a new value from:
	* the key passed as a parameter, that should not be in the map
	* the lambda to compute the mapping from the key

* The computeIfPresent() version

```
// On Map
V computeIfPresent(
	K key, BiFunction<? super K, ? super V, ? extends V> remapping);
```

* Computes a new value from:
	* the key passed as a parameter, that should be in the map
	* the existing value, cannot be null
	* the lambda to compute the remapping from the key and the existing value

* All the compute*() methods return the value
	* that has just been computed
	* or that was in the map before
* Useful to build maps of maps (for instance)

```
Map<String, Map<String, Person>> map = ... ;

// key, newValue
map.computeIfAbsent(
	"one",
	key -> new HashMap<String, Person>()
).put("two", john);
```

* All the compute*() methods return the value
	* that has just been computed
	* or that was in the map before
* Or to build maps of List (for instance)

```
Map<String, List<Person>> map = ... ;

// key, newValue
map.computeIfAbsent(
	"one",
	key -> new ArrayList<Person>()
).add(john);
```

* The last one is the merge() method, to merge maps

```
// On Map
V merge(
	K key, V newValue,
	BiFunction<? super V, ? super V, ? extends V> remapping);
```

* If the passed key is not in the map: adds the key/value pair to the map
* If the passed key is in the map
	* merge the existing value with the passed value using the lambda expression
	* note that the remapping takes a pair of values and return a new value

* Let us see an example:merge the key/values from map2 into map1

```
Map<City, List<Person>> map1 = new HashMap<>();
Map<City, List<Person>> map2 = new HashMap<>();

map2.forEach(
	(key, value) ->
		map1.merge(
			key, value,
			(existingPeople, newPeople) -> {
				existingPeople.addAll(newPeople);
				return exisingPeople;
			}
		)
);
```