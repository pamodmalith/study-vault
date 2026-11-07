---
parent: "[[Inheritance]]"
previous: "[[Overloading]]"
next: "[[Abstraction]]"
---
- Polymorphism is the ability of an object to take on many forms.
- It is a core concept of OOP that allows one interface to be used for a general class of actions.
- The most common use of polymorphism in OOP occurs when a parent class reference is used to refer to a child class object.

- Polymorphism is typically categorized into two types
	- Compile time polymorphism / Static polymorphism
	- Runtime polymorphism / Dynamic polymorphism

	 ```java title=test.java hl=7,8
	public class Animal {}
	
	public class Elephant extends Annimal {}
	
	public class Test {
		public static void main(String[]  args) {
			Elephant elephant = new Elephant(); // A Elephant Elephant
			Animal animal = d; // A Elepahnt IS-A IS-A Animal
		}
	}
	```

#### Compile-time Polymorphism (Static Polymorphism)

> [!info] Definition
> Compile time polymorphism in Java occurs when a method call is resolved at compile time. It is achieve through method overloading.

>[!info] Method Overloading
>Method overloading allows multiple method with the same name in a class but with different parameters lists. the compiler decides which metho to invoke based on the method signature.

##### Key Points
- The return type of the methods can be different, but it is not considered for method resolution.
- the number, type, and order of parameter must differ.

#### Runtime Polymorphism (Dynamic Polymorphism)

>[!info] Definition
>Runtime polymorphism occurs when a method call is resolved at runtime. It is achieved through method overriding.

>[!info] Method Overriding
>Method overriding allows a subclass to providea specific implementation of a method already defined in its parent class.

##### Key Points
- The method in the subclass must have the same name, return type, and parameters as the parent class.
- It is used to implement dynamic method dispatch, where the method that gets executed depends on the runtime type of the object.

#### Key differences between Compile time and run time polymorphism

- [n] This will be enough for exam

|        Features         |  Compile time polymorphism  |   Runtime polymorphism    |
|:-----------------------:|:---------------------------:|:-------------------------:|
|     Acheved through     |     Method overloading      |     Method overriding     |
|         Binding         |  Resolved at compile time   |    Resolved at runtime    |
|       Flexibility       |        Less flexible        |       More flexible       |
|       Performance       | Faster (no rntime overhead) | Slower (runtime overhead) |
| Inheritance requirement |        Not required         |   Required inheritance    |

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