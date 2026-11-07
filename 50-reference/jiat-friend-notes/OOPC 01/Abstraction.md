---
parent: "[[Inheritance]]"
previous: "[[Polymorphism]]"
next:
---
Abstraction is a core concept of OOP that focuses on *hiding implementation details* and showing only the essential features of an object or system. It allows developers to focus on `what an object does` rather than `how it does it`.

##### Key Features of Abstraction
- *simplifies code* : By exposing only relevant information, abstractions reduces complexity.
- *Encourages reusability* : Abstracted components can be reused in different contexts without needing to understand their implementation.
- *Increases security* : Implementation details are hidden from the user, reducing the chance of misuse.
- *Promotes Maintenance* : Changes in the implementation do not affect other parts of the code, making the system easier to maintain.
<br>
- An abstract class can have methods with and without implementation
	 ```java title=A.java
	public abstract class A {
		abstract void m();
	
		void m1() {
			System.out.println("m1");
		}
	}
	```

- Objects cannot be created from a abstract class.
	- To use a abstract class first, it must be extended to a concrete class and override abstract methods.
		 ```java title=B.java
			public class B extends A {
				@Override
				void m() {
					System.out.println("m()");
				}
			}
	
			public class Test {
				public static void main(Strings[] args) {
					A b = new B();
				}
			}
		```

	- Or with a anonymous class
	  ```java title=Test.java
		public class Test {
			public static void main(String[] args) {
				A a = new A() {
					@Override
					void m() {
						//...
					}
				}
			}
		}
		```

### How Abstraction is Achieved in Java

In Java, abstraction can be achieved using:
- Abstract classes
- Interfaces

#### Abstract Classes

An abstract class is a class that is declared with the `abstract` keyword. It can have both *abstract methods (methods without a body)* and *concrete method (methods with a body)*
##### Key Points

- Abstract classes cannot be instantiated.
- They are used as a base class for other classes.
- Subclasses must implement all abstract methods unless the subclass is also declared abstract.

#### Interfaces
An interface is a completely abstract class that contains only *abstract methods* by default (in earlier version of Java). Starting from Java 8, interfaces can also have *default methods* (methods with a body) and *static methods*

##### Key Points

- Interfaces are implemented using the *implements* keyword.
- A class can implement multiple interfaces, supporting multiple inheritance.
- Methods in interfaces are implicitly *public* and *abstract
- Variables in interfaces are implicitly *public, public* and *final*.

- There are 3 types of methods that can be created inside an interface
	- Before Java 8, only abstract methods were supports
	- After Java 8, it supported creating concrete methods using `default` modifier
	- And also static methods are supported after Java 8
- Variables can be created inside interfaces. 
	All of them are *public, static* and *final* by default. And they should be initialized (because no default value will be because)
	
	 ```java title=A.java
	public interface A {
		int a = 0;
		public int  b = 0;
		final int  c = 0;
		public final int d = 0;
	}
	```

	![[single-multileverl-hierarchical-inheritance.svg]]
		
	code examples
	
	![[multi-hybrid-inheritance.svg]]

> [!important]
> *multiple* and *hybrid* inheritance cannot be implement in Java using classes and abstract classes. They can be only achieved through interfaces.
###### Simple Inheritance

```java
class A {}

class B extends A {}
```

###### Multilevel Inheritance

```java
class A {}

class B extends A {}

class C extends B {}
```

###### Hierarchical Inheritance

```java
class A {}

class B extends A {}

class C extends A {}
```

###### Multilevel Inheritance

```java
interface A {}

interface B {}

class C implements A, B {}
```

###### Hybrid Inheritance

```java
interface A {}

interface B extends A {}

interface C extends A {}

class D implements B, C {}
```
### Advantages of Abstraction

- *Improved security* : Hides sensitive implementation details.
- *Simplifies complex systems*: Users interact only with essential features.
- *Enhances code reusability* : Abstract components can be reused across projects
- *Improved maintainability*: Implementation changes don't affect the interface

--- start-multi-column: ExampleRegion1  
```column-settings  
number of columns: 2
border: disabled
shadow: off
```

> [!previous]
> `=this.previous`

--- end-column ---

> [!next]
> `=this.next`

--- end-multi-column