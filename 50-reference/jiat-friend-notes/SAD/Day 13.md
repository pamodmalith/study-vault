---
parent: 
previous: "[[Day 12]]"
next: "[[Day 14]]"
---
> [!lecture]
> Day 13
  
> [!resources]
> 

![[j dialog - data-retrieval-getter-setter.png]]

#### Casting
There are 2 ways of casting
1. Up casting
2. Down casting

##### Upcasting
- [n] Upcasting - Super Class S = new ChildClass();

- We don't to say explicitly say to cast (implicit casting)
- This goes along with polymorphism. Therefor we are limited to attributes only n the super class.
	That means attributes specific to child class get restricted.

```java error=7 hl=23,24,25
public class Casting Demo {
	public static void main(String[] args) {
		Animal d = new Dog; //up casting
		d.makeSound();
		
		// due to compile time polymorphism
		// d.bark();
	}
}

class Animal {
	public void makeSound() {
		System.out.println("Animal: makeSound()");
	}
}

class Dog extends Animal {
	@Override
	publi void makeSound() {
		System.out.println("Dog:makeSound()");
	}

	public void bark() {
		System.out.println("Dog: bark()");
	}
}
```

##### Down Casting
- [n] Child Class c = (ChildClass) supertypereference;
- Down casting is used to keep super type reference is as a sub class type.
- In this we have to explicitly say to cast (explicit casting)
- In down casting attributes not found super can be accessed

```java hl=5,6
public class Casting Demo {
	public static voi main(String[] args) {
		Animal a = new Dog; //up casting
		Dog d = (Dog) a; //down casting
		d.makeSound();
		d.bark();
	}
}
```

```java
public MyDialog(JFrame parent, boolean modal) {
	super(parent, modal);
	Home h = (Home) parent;
}
```

- If the reference given to cast not the sub class type expecting, an  *ClassCastException*  is thrown
- To avoid this exception we can use *instanceof* keyword

```java
{
	Animal a new Dog(); // upcasting
	if (a instanceof Dog) {
		Dog d = (Dog) a; //down casting
		d.makeSound();
		d.bark();
	}
}
```
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